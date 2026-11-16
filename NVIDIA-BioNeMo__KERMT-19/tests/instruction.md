# Issue

**Title:** Fix five pipeline-correctness bugs

## Description

## Summary

Five correctness fixes found during a focused review of the training, finetuning, and inference pipelines. Every fix was verified in-container (torch 2.9.1) before committing. No refactors, no behavior changes beyond the bug being corrected.

## Fixes (one per commit)

1. fix(predict): filter invalid SMILES before featurization `task/predict.py` built `valid_indices` as `list(range(len(test_data)))`, i.e. it never filtered — the None-insertion/realignment code below it was dead. Any unparseable or empty SMILES (or a zero-heavy-atom molecule) in a prediction CSV would crash featurization instead of being skipped. Now validity is checked with RDKit (`MolFromSmiles`, ≥1 heavy atom) exactly as the training path does, so invalid rows are dropped and predictions align to the surviving molecules.

2. fix(finetune): zero MTLLoss.log_sigma grad each step (--use_mtl_loss) `MTLLoss.log_sigma` is a standalone nn.Parameter appended directly to the optimizer (param_groups[1]); it is not a submodule of `model`, so the per-batch `model.zero_grad()` never cleared its gradient. The gradient accumulated across every batch, corrupting the learned task-uncertainty weights. Added `mtl_loss.zero_grad()` alongside `model.zero_grad()`. Verified with the real MTLLoss: log_sigma.grad went 1→2→3 across steps before the fix, constant per-step after.

3. fix(eval): use BCE on probabilities for classification eval loss In eval the model applies sigmoid to classification outputs, so the eval predictions are already probabilities. The eval-loss path then passed them to BCEWithLogitsLoss, which sigmoids a second time — a double sigmoid that produced a wrong loss (off by up to ~7x on confident predictions). Now the classification branch uses plain BCE on the probabilities; regression is untouched. Verified the corrected loss matches BCEWithLogits on the raw logits to within 1e-7.

4. fix(eval): keep per-task alignment when a task has no valid targets In `evaluate_predictions`, a task with zero valid targets hit `continue` without appending to `results`, so `results` came out shorter than num_tasks and every later task's metric silently shifted down one index. Now appends float('nan') before continue, matching the all-0s/all-1s branch directly above it.

5. fix(finetune): don't double-wrap DataLoader for ensemble_size > 1 The training loop assigned the wrapped loader back to `train_data`, the same name holding the underlying dataset. The first ensemble model was fine, but the second iteration then did DataLoader(DataLoader(...)), which crashes with "'DataLoader' object is not subscriptable". Use a separate `train_loader` local and keep `train_data` as the dataset. Verified with a 2-iteration repro (old crashes on iter 1, fixed runs both).

## Behavior impact (what changes)

- Predict (#1): CSVs with invalid SMILES now run to completion instead of crashing; output rows correspond to the valid molecules only.
- Finetune with --use_mtl_loss (#2): the multi-task uncertainty weights now train correctly. Any prior --use_mtl_loss run had corrupted task weighting. --use_mtl_loss is off by default.
- Classification eval/val loss value (#3): now correct. Metric-based (AUC) selection and predictions are unchanged; loss-based checkpoint selection and HPO for classification now see the correct loss.
- Multi-task eval with partially-labeled splits (#4): per-task metrics are now correctly aligned. Single-task and fully-labeled multi-task runs unaffected.
- Ensemble finetune (#5): --ensemble_size > 1 now works; it was previously broken and could not have completed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85f255fd00b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
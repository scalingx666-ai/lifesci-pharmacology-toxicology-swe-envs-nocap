# Issue

**Title:** fix(predict+pretrain): rdkit2D predict arg, issue #22, and atom/bond-mode DDP + float-loss fixes

## Description

## Summary

Two related correctness fixes in the prediction / evaluation path:

1. **`main.py predict` rejects `--rdkit2D_normalization_type`** — the predict command couldn't use rdkit-2D-normalized features.
2. **Bond dropout is silently disabled after the first validation (and desyncs across DDP ranks)** — .

Both are small, self-contained changes with no behavior change for existing correct runs.

## Fix 1 — accept `--rdkit2D_normalization_type` on the predict parser

`predict` can build rdkit-2D-normalized features (`rdkit_2d_normalized_cuik_molmaker`), whose featurization reads `args.rdkit2D_normalization_type` (`kermt/data/molgraph.py`) and must match the normalization baked into the checkpoint. The flag was defined only on the **finetune** parser, so any predict run passing it failed with:

```
main.py: error: unrecognized arguments: --rdkit2D_normalization_type descriptastorus
```

Added the argument to `add_predict_args`, mirroring the finetune definition (choices `fast`/`best`/`descriptastorus`, default `fast`).

## Fix 2 — don't disable bond dropout on the shared `args` ()

`predict()` set `args.bond_drop_rate = 0` on the **shared** `args` instance. Training and evaluation reuse one `args` object, and graphs are rebuilt every epoch (caching off by default), so after the first validation pass bond dropout stayed disabled for **every subsequent training epoch**. Under DDP, only rank 0 evaluates, so **rank 0 trained with `bond_drop_rate=0` while the other ranks kept the configured rate**, desyncing augmentation across ranks.

Fix: shallow-copy `args` before overriding `bond_drop_rate`, so the override is local to the evaluation call and never leaks back into training or other ranks. Applied the same fix to `fingerprint.do_generate`, which had the identical pattern (impact there is benign since it's a standalone command, fixed for consistency).

## Changes

| File | Change |
|---|---|
| `kermt/util/parsing.py` | Add `--rdkit2D_normalization_type` to the predict parser |
| `task/predict.py` | Shallow-copy `args` before disabling bond dropout (+ `import copy`) |
| `task/fingerprint.py` | Same shallow-copy fix in `do_generate` (+ `import copy`) |

## Testing

- `tests/integration/test_pretrain_finetune.py::test_pretrain_ddp_finetune` — **now PASSES** end-to-end (pretrain → finetune → predict). This test previously failed at the predict step on the unrecognized `--rdkit2D_normalization_type` argument.
- Verified the predict parser accepts `--rdkit2D_normalization_type descriptastorus`, and that both `predict()` and `fingerprint.do_generate()` no longer mutate the caller's `args`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `408b86344cb9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
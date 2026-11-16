# Issue

**Title:** feat: optional multi-GPU (DDP) finetuning

## Description

## Summary

Adds opt-in data-parallel (DistributedDataParallel) finetuning so finetunes can be accelerated across multiple GPUs, mirroring the existing pretraining DDP path. Single-GPU finetuning is unchanged and remains the default — DDP only
engages through a dedicated launcher.

## What's new

**Core (`feat(finetune): add optional multi-GPU DDP training`)**
- `finetune_ddp.py`: a one-process-per-GPU launcher (torch.multiprocessing spawn, `WORLD_SIZE` env, NCCL topology auto-config, per-rank determinism), routed through the normal `cross_validate -> run_training` finetune path.
- `kermt/util/ddp_utils.py`: shared `ddp_setup` + `configure_nccl_for_topology` so the pretrain and finetune launchers use one DDP bootstrap.
- `task/train.py` (`run_training`/`train`): thread `rank`/`world_size`; wrap the model in DDP (`find_unused_parameters=True`, since the finetune model carries parameters the FFN forward does not exercise); shard the train loader with a `DistributedSampler` (+ `set_epoch`); run validation on the unwrapped module over the full val set on every rank so best-model selection stays consistent; gate checkpoint saving, logging and test-set evaluation to rank 0; all-reduce the standalone `MTLLoss.log_sigma` gradient. Also uses a distinct `train_loader` instead of reassigning `train_data`, fixing an `ensemble_size>1` DataLoader double-wrap.
- `cross_validate`: thread `rank`/`world_size`; gate W&B + reporting to rank 0.
- `build_lr_scheduler`: world-size-aware `steps_per_epoch`.

**Skill (`feat(kermt-finetune skill): expose optional multi-GPU DDP finetune`)**
- `agent/scripts/run_finetune_local.py`: new `--num-gpus N` option. Default 1 (single-process `main.py finetune`, unchanged); `N>1` launches `finetune_ddp.py` with `WORLD_SIZE=N`. Under DDP it omits the single-device `--gpu` flag and records `num_gpus`/`distributed` in `run.json`.
- `agent/skills/kermt-finetune/SKILL.md`: documents `--num-gpus` in the hardware, arguments, invocation-template, and error-hint sections.

## Design

- **Opt-in, zero-impact default.** DDP activates only when a process group is initialized (i.e. via `finetune_ddp.py`). `python main.py finetune ...` and `--num-gpus 1` behave exactly as before.
- **Per-GPU batch size.** `--batch_size` is per process; effective global batch is `batch_size * WORLD_SIZE`. The LR schedule is world-size-aware, so the same `--batch_size` can be reused as GPUs are added.
- **Rank 0 owns side effects.** Logging, checkpointing, and test-set evaluation run on rank 0; saved checkpoints are ordinary (non-DDP) state dicts, portable to single-GPU inference/finetune.

## Usage

```bash
# data-parallel finetune across all visible GPUs (or set WORLD_SIZE=N)
python finetune_ddp.py --data_path train.csv --separate_val_path val.csv \
    --separate_test_path test.csv --checkpoint_path <ckpt> --save_dir <dir> \
    --dataset_type regression --metric mae ...   # usual finetune flags, no subcommand

# via the finetune skill runner
python agent/scripts/run_finetune_local.py --ckpt <ckpt> \
    --prepare-manifest <prepare_data.json> --dataset-type regression \
    --out <run-dir> --num-gpus 4
```

## Testing

- Single-process finetune (`main.py finetune`) runs unchanged end-to-end (2-epoch regression finetune from a pretrained checkpoint): trains, selects best by validation, saves checkpoints, evaluates on test.
- DDP finetune path runs end-to-end (`finetune_ddp.py`, `WORLD_SIZE=1`): full DDP wrap, DistributedSampler + `set_epoch`, unwrapped-module eval, rank-0 checkpoint/test — completes with comparable validation/test metrics to the single-process run.
- Skill runner: `--num-gpus 1` dispatches to `main.py finetune`; `--num-gpus N` (N>1) dispatches to `finetune_ddp.py` with `WORLD_SIZE=N` (verified by dry-run command inspection and an actual launch). Existing finetune-runner unit tests (19) pass.
- Recommended before production reliance: a multi-GPU (2+) run to validate cross-rank gradient all-reduce and wall-clock scaling, which single-GPU CI cannot exercise.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc0dae393c15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Add atomic checkpoint saves for finetuning

## Description

Use .tmp + os.replace() pattern in save_checkpoint and save_model_for_restart to prevent corrupted checkpoints if the process is killed mid-write.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5603d42382e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** set up actions

## Description

I think we should leave the `--as-cran` flag, as it seems to be checking extra stuff. The `skip_on_cran()` function is not affected by the flag, I think it only looks whether it is actually running on a CRAN server or not.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60583a09e2f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
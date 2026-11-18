# Issue

**Title:** RXR-973: Use isTRUE when checking whether to build

## Description

Minor update to our logic for checking the `build` field.  If the `build` field is missing, `model_from_api()` would error. I don't think we necessarily want to require this field in the public versions of our models, so I'm using `isTRUE()` to avoid the error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee137d8990f6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** RXR-927 allow rounding to be specified in new_covariate()

## Description

This adds a feature to perform rounding of covariate times. This is especially useful in production code when assuming a certain precision for regimens, TDMs, and covariate times.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da55a3fa2765`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
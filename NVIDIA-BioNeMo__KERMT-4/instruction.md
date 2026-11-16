# Issue

**Title:** Fix TypeError when dynamic_depth="truncnorm" by using rvs() instead of rvs(1)

## Description

scipy's truncnorm.rvs(1) returns a 1-d NumPy array, which int() cannot convert to a scalar. Using rvs() without a size argument returns a scalar directly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d4e4f2cc0bd2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
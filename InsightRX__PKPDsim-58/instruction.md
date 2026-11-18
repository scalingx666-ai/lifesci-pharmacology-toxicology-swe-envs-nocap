# Issue

**Title:** RXR-1228 fix bug when joining regimens with no t_inf

## Description

When a drug is oral or SC administered, it will not have a t_inf specified. This PR fixes the behavior and adds a test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `acffc2cf150e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
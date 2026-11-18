# Issue

**Title:** RXR-3049: small bug when interval = infusion length

## Description

Fixes a bug where we tried to assign values to a row of length 0, in some cases (interval = infusion length); also removes mockery from tests (deprecated package)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82dad97b975e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
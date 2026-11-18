# Issue

**Title:** RXR-1050: Fix github actions

## Description

pkgdown started failing after #46, I updated to the latest version of the `setup-r` and `setup-r-dependencies` actions and that seems to have resolved it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bf675cc47212`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
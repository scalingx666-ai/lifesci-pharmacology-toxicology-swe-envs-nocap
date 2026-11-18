# Issue

**Title:** RXR-1851: add function to calculate auc pre-ss

## Description

This adds a convenience function to calculate AUC (and conc) using linear models, based on just the PK parameters.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac52a0cdc253`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
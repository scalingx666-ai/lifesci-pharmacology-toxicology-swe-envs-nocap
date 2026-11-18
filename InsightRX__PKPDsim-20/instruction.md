# Issue

**Title:** RXR-602 more fixes to lagtime implementation

## Description

We have to supply cmt_mapping that is tied to the model as well, otherwise lag times are not supplied to the correct compartments

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06f628b77aed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
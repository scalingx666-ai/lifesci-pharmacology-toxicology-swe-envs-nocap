# Issue

**Title:** Updates on parallel bitbirch

## Description

- Modifications to fit_BFs and prepare_data_BFs:
Now the BitFeatures as stored as lists instead of _BFSubcluster objects. This with the objective of saving memory for larger instances. The fit_BFs was modified to fit this format. 

- Parallel bitbirch example
The notebook, and related scripts were modified to adapt to the new parallel strategy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2cac79ac5ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
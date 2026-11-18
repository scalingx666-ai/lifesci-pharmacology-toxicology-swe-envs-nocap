# Issue

**Title:** RXR-1298: Include development info in installed package

## Description

#60 included the development info in the object returned by `new_ode_model()`, but that info wasn't included in the installed package.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21283703b37a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** RXR-1802: fix issue with parsing of a long line

## Description

when compilation of a model with a 5 different output variables, the parsing when wrong because the parser split it into multipel lines, which messed up the template `model.R` file in the generated package.

I don't think a test is necessary, simple fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97508335e231`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
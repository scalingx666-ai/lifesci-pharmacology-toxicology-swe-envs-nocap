# Issue

**Title:** CRAN submit of 1.1.1

## Description

- Added some info for CRAN reviewer
- Added NEWS
- updated suggestion from checks to update the way classes are checked (using `inherit()`) 
- removed the stripping of the compiled files, since that seems to be a violation of CRAN policy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8b0126658da3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
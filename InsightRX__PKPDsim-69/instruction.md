# Issue

**Title:** RXR-1379: require fixed parameters in model definition

## Description

This reverses the changes in #66; we now require `"fixed"` to be present in the model definition.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a882732bde41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
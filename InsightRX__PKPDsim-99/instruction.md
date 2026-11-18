# Issue

**Title:** helper functions for working with PKPDsim models

## Description

A lot of information about a model is hidden within its attributes, which are cumbersome to access via `attr` or `attributes`.

By wrapping `attr` calls for typical fields we access into functions, this information is made more accessible and better documented.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53fa1730641b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
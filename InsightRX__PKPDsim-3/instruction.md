# Issue

**Title:** Fix comment feature

## Description

Last pull request failed when adding comments.  

Tested with trace in R and appears to successfully remove comments from 'code' input of new_ode_model now.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d312607e85fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
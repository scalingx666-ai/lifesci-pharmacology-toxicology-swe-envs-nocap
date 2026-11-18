# Issue

**Title:** RXR-982: Run check in correct directory

## Description

The PKPDsim source code is in `/src` but the latest irx-r-base image changes the working directory to something else, so I've updated the jenkinsfile here to run `devtools::check()` with the absolute path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8b0126658da3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
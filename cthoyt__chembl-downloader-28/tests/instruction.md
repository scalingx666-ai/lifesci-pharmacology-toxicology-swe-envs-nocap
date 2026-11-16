# Issue

**Title:** Refactor logic for extracting SQLite database from a TAR archive

## Description

This PR refactors logic for extracting SQLite database from a TAR archive such that it will deterministically look to see if the archive has already been unpacked. This can save a lot of space, in case you go back and delete the original.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9efc6ccc5a15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
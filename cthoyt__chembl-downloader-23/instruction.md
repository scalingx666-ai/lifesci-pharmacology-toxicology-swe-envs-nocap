# Issue

**Title:** Add shortcut for querying for a scalar

## Description

Add a function that wraps `query()` for SQL queries that only return one row/column, and automatically unpack them.

This PR also adds unit testing which mocks a ChEMBL database (with sneaky usage of PyStow mocks)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff58799c4f84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
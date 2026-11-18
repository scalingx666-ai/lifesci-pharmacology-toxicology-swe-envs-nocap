# Issue

**Title:** Fix handling of rate in regimen_to_nm() function for iv + bioavailability

## Description

See updated documentation for explanation.
(Fix is not relevant for production, this is only changes to a function to convert regimens to NONMEM data format)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73da416fe90d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
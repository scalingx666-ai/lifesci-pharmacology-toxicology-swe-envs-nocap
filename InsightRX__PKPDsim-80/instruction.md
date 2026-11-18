# Issue

**Title:** RXR-1771: Remove constraints on regimen type

## Description

Removes logic that forces `type` to be one of 'bolus', infusion', 'oral', 'sc' or 'im'. If `type` is `NULL` we will still assume it's infusion if there's a `t_inf`, bolus otherwise.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d214c97d0c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** RXR-1771: Include sc, im among types whose t_inf should be 0

## Description

We were getting errors for a regimen that was a mix of bolus and sc doses. `t_inf` for the boluses was being set to 0, but for sc it was `NA`. This led to an error at line 129. I made two changes: added sc and im to the vector of types whose infusion length should be set to 0. Also added `na.rm = TRUE` at line 129 for greater safety.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e23960fc8988`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
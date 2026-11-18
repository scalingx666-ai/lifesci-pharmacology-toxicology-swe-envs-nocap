# Issue

**Title:** RXR-2590: add support for additional "types" of oral dosing

## Description

`new_regimen()` will now use regex to look for the word "oral" instead of requiring an exact match to the word "oral", allowing the same t_inf adjustments for any type with "oral" in it (e.g., "oral_tablet", "oral_suspension").

I also noticed that we had conflicting logic for SC and IM dosing, where in `new_regimen()` we explicitly test for non-0 infusions for SC while in `create_event_table()` we forced these to be 0. I resolved this conflict by allowing SC and IM dosing to have non-zero t_inf.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `639b5b383a3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
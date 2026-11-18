# Issue

**Title:** ensure if always has length 1 result

## Description

One more issue arose with the updates to regimen_to_nm in which we had a condition length > 1 if we allowed for multiple dose compartments within one regimen (since the `if` statement condition would be of length > 1).

I accidentally also committed some changes I was making to get_t_obs_from_regimen to update the code base to no longer assume all infusion regimens were of type "infusion". I think it's okay to bring them in too, but let me know if you want me to separate them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `90f8a84e0cd4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
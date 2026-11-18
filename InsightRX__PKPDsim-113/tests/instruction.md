# Issue

**Title:** fix handling of dose-dependent parameters

## Description

In the core `sim()` function there was an edge case where dose-dependent variables or parameters were not correctly initialized, being either NA or a wrong value. Dose-dependent variables are usually implemented using the built-in `prv_dose` variable that updates whenever a new dose is given. For the first initialization step (before looping through the event record), this was previously getting set to the first value in the `dose` column of the event record. But the first row of the event record is never a dosing record, but an initialization row to make sure variables are correctly set, so it will never hold the value of the dose. Therefore the `prv_dose` value upon initialization was always set to 0.

This fix improves that by finding the first dose record (evid == 1) in the event record, and using that value to initalize.

Technically it would be slightly better to use the first dose record only if the dose is at t=0 because the user may expect that `prv_dose` stays 0 until a dose is given, but that is currently not possible due to the way lag-time is implemented. That will be addressed in a later PR.

See also  for more details / thoughts, and description of another minor issue with current lagtime implementation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fe24fb40723`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
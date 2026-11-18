# Issue

**Title:** RXR-1042 allow sc dosing in new_regimen()

## Description

new_regimen() only allowed `oral`, `infusion`, and `bolus` dosing. It should also allow `im` and `sc` as potential administration types / routes. There is no additional logic required, i.e. any logic on where to put doses in the ODE system should be added in the model definition itself using `cmt_mapping`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35c65a96a3e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
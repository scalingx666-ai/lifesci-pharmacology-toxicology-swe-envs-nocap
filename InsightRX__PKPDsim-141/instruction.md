# Issue

**Title:** Update logic for setting rate in model, when exporting regimen to NONMEM-style data.frame

## Description

This fixes the behavior in the function that created NONMEM-style datasets from PKPDsim regimens. The earlier  was implemented to support models with infusion rates AND bioavailability specified. I believe the use-case for this was some oral models with zero-order absorption. In those cases, we need to make a small adjustment to the NONMEM model, and specify rate in the model itself, to properly account for bioavailability. However, for the majority of models (iv infusions) this is not needed. If `bioav` was set to 1 the logic shouldn't fire, and rate shouldn't have to be defined in the NONMEM model. This logic was now added.

**Note**: This PR doesn't touch any production code, it is just in a convenience function that is used in irxmodval.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8a0ef512420`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
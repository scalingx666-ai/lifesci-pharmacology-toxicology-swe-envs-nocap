# Issue

**Title:** RXR-2593: Ensure covariate times are correct when t_init is non-zero

## Description

We allow the argument t_init to be specified in the case of observations prior to the first dose. When this occurs, we shift the regimen start time to begin at t_init, effectively changing the "origin". However, we were not also shifting covariate times. As a result, there could be a mismatch in covariate times and dose times in simulation.

I opted to call PKPDsim::new_covariate again to update the covariate times since this ensures the covariate values are correctly extrapolated backwards to t = 0, respecting existing implementation method for extrapolation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5524d2d2df9f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
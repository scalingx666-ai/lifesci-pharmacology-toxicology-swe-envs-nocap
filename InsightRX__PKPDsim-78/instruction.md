# Issue

**Title:** RXR-1751 add safety

## Description

When duplicate names are used in `covariate` and `declare_variables` arguments, the covariates are not exported in the output column. Example to see this:

```
library(PKPDsim)
library(pkvoriconazolefriberg)
mod <- pkvoriconazolefriberg::model()
covs <- list(
  WT = new_covariate(70),
  AGE = new_covariate(70),
  CYP2C19unknown = new_covariate(1),
  CYP2C19a1a2 = new_covariate(0),
  CYP2C19a2a2 = new_covariate(0),
  CYP2C19a2a3 = new_covariate(0),
  CYP2C19a1a3 = new_covariate(0),
  CYP2C19a3a3 = new_covariate(0)
)
reg <- new_regimen(amt = 500, interval = 12, n = 5)
res <- sim(
  mod,
  covariates = covs,
  regimen = reg,
  parameters = pkvoriconazolefriberg::parameters(),
  output_include = list(variables = TRUE, covariates = TRUE)
)
res[1:5,]
```

After this change it's now impossible to specify duplicate covariates and variables in model specs. I initially implemented code in `sim()` that checks for duplicates in the output of sim() and made the parsing safer, but I removed that because it should now be unnecessary.

- Added a test for throwing of the error
- removed PKPDsim.Rproj (unrelated cleanup)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f14e1cb3927`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
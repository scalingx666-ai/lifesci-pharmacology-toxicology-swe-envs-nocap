# Issue

**Title:** RXR-792: updates to allow double absorption pazopanib

## Description

Some absorption models are implemented using double dosing compartments with different absorption rates, each possibly having a lag time as well. Example is the pazopanib model by Yu et al, but I have seen it before in other models as well. Some adjustments to the regimen have to be made, essentially just a duplication of the regimen and adjustment of the compartments. However, we should implement a feature so this can be defined in model metadata, and ensure no other action is required in module metadata (since this is really a "model" feature and not a "module" feature).

For the pazopanib model specifically, a separate other fix in PKPDsim is required too: the model defines the bioavailability of the absorption compartments using the previous dose:
TVB  = (1-(prv_dose-200)*EMAX/(ED50+prv_dose-200))
However, this fails for the first dose, because the `prv_dose` variable is only initiated after the first row of the event record during simulation. So the correct value for biovailability does not get applied during the first dose. This is a simple fix in sim.cpp.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b2cc572ee72c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
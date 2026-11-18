# Issue

**Title:** RXR-1470: circumvent difference in how NONMEM and PKPDsim handle bioavailability for infusions

## Description

PKPDsim automatically applies the bioavailability to infusions. NONMEM doesn’t do that, it instead recalculates duration as ((AMT/bioav)*rate), and then uses the given RATE to infuse. So for bioavailability of 10%, the duration will be 10 times shorter. But that's generally not really what one wants. The more intuitive way is to  just multiple RATE by bioavailability.

Generally we only have a bioavailability factor for oral models, which use a bolus into a dose compartment, so this issue will never surface. However, sometimes there is a use case for IV models too, e.g. when using it to convert unit from mg to mmol, or drug to prodrug. NONMEM's behavior with this scenario is also explained in the [RxODE manual](https://nlmixrdevelopment.github.io/RxODE-manual/events.html), as RxODE matches NONMEM's behavior.

To be clear, the behavior in PKPDsim was correct, and we’re not changing that. It’s just slightly different from NONMEM / RxODE, which IMO is less intuitive. Here, we're only updating the exporter to NONMEM datasets, so it matches behavior in PKPDsim and can be used for automated validations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `098257f1354b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
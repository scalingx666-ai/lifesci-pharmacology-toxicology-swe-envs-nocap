# Issue

**Title:** move up check for parameter vs parameter table

## Description

Needed for PKPDposterior, but also a good idea in general b/c we just want to check the user-specified arguments.
This failed for the busulfan model in PKPDposterior because in the sim() function additional "kappa_" parameters get added to the `parameters` object to support IOV, so it gets updated and hence looks like we are supplying both `parameters` and `parameters_table`. In principle it's bad practice to change user-supplied arguments, but that's probably too big of a change for this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `03b393e188da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
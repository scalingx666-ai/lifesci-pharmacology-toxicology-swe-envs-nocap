# Issue

**Title:** RXR-633 rm unused functionality

## Description

This removes an earlier implementation of analytic equations, since we now have a newer advan() function that handles that. While sim_core_analytic() is in principle a somewhat more generalized version of advan(), in practice I don’t think there will be a use case for it. I checked that neither sim_core_analytic() nor analytical_eqn_wrapper() is used in any of our other R packages.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3af74cf588c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
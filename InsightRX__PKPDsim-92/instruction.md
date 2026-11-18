# Issue

**Title:** RXR-1926 implement infusion duration scaling

## Description

This implements a feature where a model automatically scales infusion durations by a specified factor.
An example model where this is used in NONMEM is Centanni et al. CPK 2024.

In the implementation in PKPDsim it is handled in the same was as lagtime, it just modifies the input regimen / event table that is used in the simulation function.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `66bb556e5fbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** RXR-799: parse bioav as array

## Description

In  we made changes to allow for multiple absorption compartments, which necessitated allowing bioavailability to be specified as a vector. This PR extends `new_ode_model` to allow bioavailability fields in model JSON files (or other methods of specifying a model) to be provided as a vector.

Only one line really needed to be changed (see commit ); the rest of the lines of code changes were to enable better unit testing and readability.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98163c62e8a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
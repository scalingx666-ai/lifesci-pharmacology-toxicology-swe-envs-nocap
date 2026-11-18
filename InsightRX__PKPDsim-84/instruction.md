# Issue

**Title:** busulfan models: mccune and shukla

## Description

This PR adds two new models to our open source model library: the McCune model and the Shukla model. Both models have been previously published (citation in model files) and were used as part of a comparison of NCA and MAP approaches to AUC estimation presented at IATDMCT 2022 (Prague).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97508335e231`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
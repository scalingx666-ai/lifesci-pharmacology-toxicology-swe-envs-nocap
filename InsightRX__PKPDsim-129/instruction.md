# Issue

**Title:** RXR-2907: add new models for open access, fix misnamed fields

## Description

We recently wrote a manuscript reporting model performance across a few adult models. We should make this code available for transparency. This PR adds the model specification used for these models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c109c07f9c5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
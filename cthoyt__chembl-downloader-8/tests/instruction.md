# Issue

**Title:** Add SQL for getting activities by target

## Description

This PR adds some functionality for generating target-based datasets, motivated by 

See the notebook [here](https://nbviewer.org/github/cthoyt/chembl-downloader/blob/a68bef1c12a8bb53f78687ee8af4971de32c01ba/notebooks/refresh-static-data.ipynb) (note that this is pinned with a permalink to the state after merging this PR).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbe029e750a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
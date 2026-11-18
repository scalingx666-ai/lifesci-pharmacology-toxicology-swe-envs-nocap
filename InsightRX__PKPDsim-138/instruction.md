# Issue

**Title:** RXR-3174: safety check to ensure max sim time is reasonable

## Description

When trying different parameter estimates during MAP Bayesian optimization, it is possible to achieve a TLAG estimate that is very very large. In this case, it is possible that the vector of timepoints simulated becomes very, very large.

In theory, we shouldn't ever need to simulate past the t_obs time point.

This PR addresses the TLAG large vector issue by limiting sim time. Tests in this repo and dependencies pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6f96c3ef5e8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
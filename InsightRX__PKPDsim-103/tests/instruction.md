# Issue

**Title:** RXR-2140: fix issue due to change in warning behavior in upcoming Rcpp

## Description

See description of .

The failure in PKPDsim test was due to an erroneously configured test case, which had the wrong number of bins for simulation of inter-occasion variability. This led to extra warnings using the new Rcpp. This PR fixes the test case. 

It also adds an extra warning to make it harder to misconfigure the number of IOV bins (and a new test to confirm the warning is thrown). I did not make that a hard stop(), because in most cases the misconfiguration is not affecting the results.

For reviewing this PR: 
1. first run tests using old version of Rcpp (anything before 1.0.12.4). The tests should all run fine, either on master or on this branch.
2. then install latests version on `Rcpp/main` using `remotes::install("RcppCore/Rcpp")`. Then rerun tests. The tests should fail on the `PKPDsim/master` branch, but not on `PKPDsim/RXR-2140-Rcpp-issue`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4bf586cc04b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** RXR-1373: prep for CRAN release

## Description

Per the email from CRAN, I've added `PKG_CPPFLAGS = -D_HAS_AUTO_PTR_ETC=0` to `src/Makevars`. If I understand the email correctly I think this should resolve the issue they emailed us about. However, I was not able to reproduce the issue using the [clang16 docker container](https://r-hub.github.io/containers/containers.html#clang16) from R-hub, so I am not certain that this will fix it on CRAN's end.

I also included a few minor fixes to things that caused NOTEs in R CMD check, and updated the DESCRIPTION file with a new version number and date.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc0349d6ec8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
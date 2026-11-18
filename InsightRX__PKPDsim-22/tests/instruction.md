# Issue

**Title:** RXR-639 reduce package size (strip libs)

## Description

Requires trick described here:

http://dirk.eddelbuettel.com/blog/2017/08/14/#009_compact_shared_libraries
https://stackoverflow.com/questions/46280628/object-files-in-r-package-too-large-rcpp

With the Makevars file, the NOTE about pkg size is not thrown anymore.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c46f6781e8a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
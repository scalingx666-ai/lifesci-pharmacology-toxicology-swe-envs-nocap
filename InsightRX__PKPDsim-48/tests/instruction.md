# Issue

**Title:** RXR-1070: fix directory of built packages

## Description

I was seeing a lot of "Package not created" messages when building our models. On investigation, this was due to a change in #28 where we we stopped changing the working directory, but didn't update the rest of the logic for where to find the built tarball. I've fixed it, plus another issue that was throwing a warning.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f833a4cbc6fa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
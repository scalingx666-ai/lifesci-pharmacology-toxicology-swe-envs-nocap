# Issue

**Title:** RXR-1052 multi obs types

## Description

Another issue with multi-obs-type models. See also previous PR here:  The fix implemented there fixed one issue but created another problem for other models that wasn't caught by current tests. This PR is a slight refactor of the first solution. Also added test to test for correctness for the specific model that was broken.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d77f4fa6845`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** SS-152: added the hot fix for checking ipython

## Description

Added the hotfix patches for:
- Mango Additions Hot Fixes and Renaming of Objects
- Jupyter Notebook fixes for Windows NT environment.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a602daacf00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Update to use pyproject.toml

## Description

Simple update to use `pyproject.toml` instead of `setup.py`, to support current pypa recommendations and modern `pip` versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e49925949c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
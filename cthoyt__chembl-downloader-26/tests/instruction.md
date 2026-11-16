# Issue

**Title:** Automate generating temporal summaries

## Description

This PR updates the `chembl_downloader history` function to make it easier to make a temporal history over chembl. Unfortunately, I don't enough hard disk space to download all versions, so this gives the option to iteratively delete old versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ff1a3649a81`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
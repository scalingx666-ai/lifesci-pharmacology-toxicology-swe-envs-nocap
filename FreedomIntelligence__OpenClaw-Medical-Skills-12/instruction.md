# Issue

**Title:** fix: correct badge URLs, add .gitattributes, and fix clone instructions

## Description

## Summary

Three fixes in one PR:

### 1. Badge URLs point to wrong organization
All shield.io badges and several links in both `README.md` and `README_zh.md` reference `MedClaw-Org/OpenClaw-Medical-Skills`, but the repo lives at `FreedomIntelligence/OpenClaw-Medical-Skills`. This causes badges to show incorrect or zero stats.

**Fixed:** 9 occurrences in `README.md` + 9 in `README_zh.md`.

### 2. Clone failure on macOS and Debian ()
The repo contains ~250MB of large binary/data files committed directly to git:
- `skills/spatial-transcriptomics-*/repo/src/db/chroma_squidpy_db/data_level0.bin` (62MB each)
- `skills/trialgpt-matching/repo/dataset/trec_*/retrieved_trials.json` (36-46MB each)
- `skills/genome-compare/data/*.gz` (5MB each)

These cause `git clone` to fail with checkout errors on macOS (APFS) and some Linux distros.

**Fixed:**
- Added `.gitattributes` to track large files with Git LFS
- Replaced the plain `git clone` instruction with a sparse-checkout method that skips large data files

### 3. Clone instructions updated
The default clone command now uses `--depth=1 --no-checkout` + `git sparse-checkout` to only download the `skills/` directory, avoiding the large data files entirely.

## Test plan

- [x] Verified sparse-checkout clone works on macOS (reproduces issue #1 without fix)
- [x] Badge URLs now point to correct org

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b188633ae16d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
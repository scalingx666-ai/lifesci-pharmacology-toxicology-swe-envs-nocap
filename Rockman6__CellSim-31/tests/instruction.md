# Issue

**Title:** Held-out HeLa test: 19 h cycle supported, 31 h rejected

## Description

HeLa's cycle time and quiescent fraction were calibrated on the CTC's **Fluo-N2DL-HeLa** movie, so matching it was never evidence. This tests against the CTC's other HeLa dataset, **DIC-C2DH-HeLa** — different cultures, different microscopy, never used for any calibration.

## The test

It spans only **14 h**, too short for a daughter to complete a cycle. But it can test what the two parameterisations disagree on: the fraction of founding cells that divide within the window.

| | founders dividing in 14 h | vs measured 14/15 |
|---|:-:|:-:|
| old HeLa, 31 h cycle | 0.56 ± 0.17 | **rejected**, p ≈ 0.002 |
| current, 19 h + 50 % quiescent | 0.75 ± 0.12 | consistent, p ≈ 0.08 |
| measured | **0.93** | — |

**Independent data rejects the old cycle time and supports the new one** — the first part of the HeLa change to be validated rather than calibrated.

## What it does NOT cover

Founders are never quiescent in the engine (that's decided at mitotic exit), so this tests the **cycle time only**. The **50 % quiescent fraction** acts on daughters beyond a 14 h window and remains untested.

The measurement also sits high even for the new model, and this field is sparse (~10 founders vs 43–125). If sparse cultures proliferate more, quiescence depends on density rather than being a fixed HeLa property. Fifteen cells can't settle that.

## Reproducible

`scripts/validate_hela_heldout.py` downloads the movie (42 MB, gitignored), scores it, and re-runs both models.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ccdfccf19cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
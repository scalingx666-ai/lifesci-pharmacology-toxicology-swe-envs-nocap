# Issue

**Title:** Apply both approved defaults, publish the live demo, and re-validate everything they touched

## Description

Henry approved three things; this applies them and re-measures what they moved.

## 1. GitHub Pages — live

**https://rockman6.github.io/CellSim-3D-Multicellular-Biology-Drug-Testing-Simulator/**

Enabled on the repo, auto-trigger restored, and verified by actually fetching it: landing page, viewer, all five demo streams and the 1.4 MB spheroid run. The deploy also confirms the pip-only claim independently — it installs with plain `pip` on a clean runner, simulates every demo and checks the viewer can read each one.

## 2. `ic50()` median-over-seeds

Default `n_seeds=3`; `n_seeds=1` restores the old behaviour. One run is a random variable with ±20 % spread set by *which resistant lineages were drawn*, not by cell count.

**It needed fixing in two places.** `scripts/validate_gdsc.py` carries its own copy of the estimator and never calls `engine.ic50`, so the first re-validation returned every fitted constant **identical to four significant figures** — the signature of a code path that never ran, not of a small effect. The duplication is the real defect and is now flagged for unification.

Re-running the full GDSC validation on medians:

| | single-seed | median-of-3 |
|---|:-:|:-:|
| engine closer on | 34 lines | **32** |
| null closer on | 39 | **41** |
| sign-test p | 0.64 | **0.35** |
| held-out class correct | 80/91 | 78/91 |
| fitted constants | — | unchanged (≤ 0.6 %) |
| wall | 23 min | 72 min |

**The comparison moved against the engine**, which is what removing a model's own noise is allowed to do — a fair comparison is the point, not a flattering one. The central negative result now rests on cleaner numbers. The constants didn't move because the bisection's tolerance was already coarser than the noise it averaged over.

## 3. HeLa split into cycle time + quiescent fraction

19 h cycle with 50 % quiescent, replacing a 31 h population figure. This needed `quiescent_fraction` moved from `Params` onto `CellLine`, since it belongs to the line, not the world; every other line carries 0 and is untouched.

Re-running the dish validation (which the change silently invalidated — see below):

| | trajectory error | complete cycle | undivided 30 h |
|---|:-:|:-:|:-:|
| seq 01 before | 0.12 | 26.4 h | 0.41 |
| seq 01 after | **0.06** | **17.0 h** | **0.52** (measured 0.51) |
| seq 02 before | 0.15 | 27.3 h | 0.45 |
| seq 02 after | **0.05** | **17.8 h** | 0.57 (measured 0.71) |

**Labelled a calibration, not a validation.** The 19 h and 0.50 were scanned against these very sequences, so "it matches the movie" would be circular. What survives: two parameters reproduce eight targets with no per-sequence tuning, and the structural claim that doesn't depend on their values — no single cycle time can give both a 46 h count trajectory and a plateauing Kaplan-Meier curve. A held-out time-lapse would settle it.

## 4. Housekeeping: stale derived artefacts

Three instances in one day of a change landing in one place while a derived artefact went on asserting the old world — `dish_validation.json` after the HeLa change, the duplicate GDSC estimator, and a C50 table quoting four significant figures after the text said it was approximate. The benchmark JSONs are outputs, so nothing fails when they go stale. `docs/PLAN.md` now records the shape and the fix (one target that regenerates them and fails on disagreement).

Also: `cellsim help` distinguishes commands that work from a plain pip install from those needing conda.

## Tests

Full cell suite + pip-install gate: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1a8108c1a8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
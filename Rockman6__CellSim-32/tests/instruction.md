# Issue

**Title:** CI: per-PR mechanism tier, nightly validation tier (44 → ~16 min)

## Description

The per-PR cell step grew **21 → 44 min** in one day and became the bottleneck on every merge.

## The split — nothing weakened

Every gate still runs; this changes **when** the slow ones run, not what they check. Rule in `tests/cell/_tiers.py`:

- **Mechanism** gates — check the code does what it claims. Fast. **Every PR.**
- **Validation** gates — check against measurements (multi-day spheroids, calibration from simulated plates). Minutes each. **Nightly** (new `cell-nightly.yml`).

| | before | after |
|---|:-:|:-:|
| per-PR tier (local) | 1247 s | **477 s**, 0 failures |
| nightly, all tiers | — | 1065 s, 0 failures |

I deliberately did *not* shrink slow tests to make them fast — that's how two tests in this suite once passed while measuring nothing.

## A correctness bug found on the way

`ic50()` medians over seeds `seed..seed+2` by default. `predict_band` iterated seeds 1, 2, 3 itself, so each point was a median over **overlapping** windows `{1,2,3}`, `{2,3,4}`, `{3,4,5}` — the band meant to show run-to-run spread was smoothed by its own averaging, at 9× the cost. Explicit-seed callers now pass `n_seeds=1`; both docstrings warn about it.

## Checked, not assumed

The nightly job uses the same conda env as `smoke.yml`: 13 cell tests reach the chemistry stack, so a pip-only setup would have failed on night one.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dc817cd7171`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
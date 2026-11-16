# Issue

**Title:** Phase 2 audit: refit the library, re-measure every claim (one retraction)

## Description

## Why

The library's fitted potency constants predated the apoptotic buffer and efflux channel (off by 1.2–2.2× against its own "reproduces within 1 %" claim). Refitting moved every Phase-2 dose relative to its drug's potency, and re-running showed which Phase-2 headlines were mechanism and which were one dose or one random seed.

## Corrections (all in `docs/VALIDATION.md`, "Phase 2: audited")

| Claim | Status | Now |
|---|---|---|
| Pulsing selects a pharmacokinetic escape, not an apoptotic one | **Retracted, backwards** | Rested on 1 surviving lineage. Measured exactly on a trait grid: continuous exposure selects on drug accumulation; pulsing shifts selection toward the apoptotic reserve (all three drugs). |
| Sequence effect is a readout artefact (0.93→0.98→0.99) | Qualified | One seed; noise ±0.02–0.03. Five seeds: none at matched IC50; ~12 % at 72 h with cisplatin at 1.25× IC50, fading to ~3 %. |
| Exposure-time ranking 242×/17×/7.6× | Replaced | Dose-dependent metric. Iso-effect curve instead: cisplatin follows C×T (slope −0.95, Ozawa 1989); paclitaxel cannot reach half kill below 24 h at any dose; Liebmann's 24 h plateau reproduced; one documented miss (24 h paclitaxel ~260× too effective vs Georgiadis 1997). |
| Antagonism 1.3–1.5× | Confirmed | 1.29–1.35 ± 0.08 at matched IC50, rising with more cisplatin. |

## Structural fixes so this cannot recur silently
- Experiments state doses as multiples of each drug's engine IC50, computed at run time (`engine.ic50`, `exposure_h`).
- Tests pin the potency their doses were designed at (an exact dose rescaling for cisplatin/paclitaxel).
- Selection is measured exactly (`cellsim/cell/selection.py`, `simulate(traits=...)`), not by averaging survivors of random draws.

## Tests
Cell suite: 29 files, 0 failures, 130 s locally.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27a17966e3e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
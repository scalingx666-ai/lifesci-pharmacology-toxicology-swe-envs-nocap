# Issue

**Title:** Phase 4: hold-out test of calibration — it failed first, and found why

## Description

## What this adds

`scripts/validate_calibration.py` asks the question Phase 4 rests on: **does a calibrated engine predict a condition it was *not* calibrated on?** Fitting a constant so the simulated IC50 matches a measured one proves nothing — the constant was chosen to make that number right.

With ground truth we control: perturb a drug's constant to a value the calibrator is never told → simulate the noisy 72 h plate those cells would give → hand over **only** the fitted IC50 and its interval → ask about a 12 h pulse washed out long before the readout → check the band contains the truth.

Also `cellsim.api.gr_curve()`, so a simulation and a measured plate are the same quantity and can be compared on one axis.

## It failed first, which was the point

The band contained the held-out truth in **3 of 7 trials**. Nothing upstream was broken: the plate fit's coverage had been measured, the calibration's propagation had been measured, each piece passed its own test. What had never been tested was the **composition** — which a hold-out catches and unit tests structurally cannot.

## Finding 1: the engine's IC50 is a random variable, and more cells don't fix it

| `het_sigma` | 24 cells/dose | 96 cells/dose |
|---|:-:|:-:|
| 0.30 (default) | **1.187×** | **1.108×** |
| 0 (identical cells) | 1.009× | 1.008× |

With identical cells the engine is deterministic to under a per cent. So the ±20 % spread is **not counting noise** — it's *which resistant lineages were drawn*, and a tail converges far more slowly than a mean. Correct biology (Spencer 2009, the observation the heterogeneity was built from), with three consequences:

1. A single-seed IC50 is good to ~±20 %; smaller differences between runs aren't differences.
2. Any calibration from one measurement inherits it.
3. **More seeds, not more cells**, is the remedy — and the codebase defaults to one seed throughout.

`predict_band` now spans the calibrated interval **and** seeds:

| | before | after |
|---|:-:|:-:|
| band contains held-out truth | 3/7 (43 %) | **16/20 (80 %)** |
| median band width | — | 0.435 surviving fraction |
| the truth's own seed spread | — | 0.173 |

About 40 % of the band is irreducible. The honest reading is *"between a quarter and two thirds survive"* — enough to rule out *nearly all die* and *nearly all live*, not enough to separate 35 % from 45 %. Two schedules whose bands overlap have not been shown to differ.

## Finding 2: a residual ~10 % bias, documented rather than removed

The fitted constant lands ~10 % low, one-sidedly — still 0.88–0.92× with measurement noise removed entirely. Two definitions of "the IC50" are in play: calibration targets where median viability is ½; `engine.ic50` interpolates a crossing on a log grid and lands ~1.1× low on a curve this steep. Chasing it would cost the optimisation that makes calibration usable, to correct something well inside the ±20 % the engine varies by anyway. Recorded with the numbers rather than silently accepted.

## Tried and rejected

Averaging the calibration criterion over seeds. The argument was sound — bisecting on one realisation chases noise — but three seeds returned **byte-identical** constants at 3× the cost, moving the offset under half a per cent, because the offset is *definitional* and averaging noise cannot reach a bias. Default back to one seed, option kept, negative result written into the docstring and `VALIDATION.md` so it isn't re-derived.

## Tests

`tests/cell/test_calibrate_smoke.py` 9 gates (adds one pinning that the band widens when seeds are included). Full cell suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d6b330b3b7f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
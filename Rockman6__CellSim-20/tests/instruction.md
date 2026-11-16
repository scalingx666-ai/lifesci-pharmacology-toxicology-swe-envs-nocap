# Issue

**Title:** Phase 4: use your own cells — read a plate, fit it, tune the engine to it

## Description

## Why

Everything validated so far uses **our** cell lines and **our** fitted constants. A lab has its own line, its own reader and its own drug, and until their numbers can enter the simulator none of this is a tool for them. This is that door.

```bash
cellsim plate --data reading.csv --map map.csv --calibrate A549 cisplatin
```

- **`cellsim/plate.py`** — reads a reader's grid export or a tidy table, normalises against the plate's own blanks and controls, fits a four-parameter curve with a bootstrap interval, and reports GR metrics (Hafner 2016) when the plate carries a time-zero read.
- **`cellsim/calibrate.py`** — turns that curve into the engine's constants: the line's doubling time from its control wells, and the drug's **one** fitted constant bisected to match the measured IC50. The measurement's uncertainty is propagated, so `predict_band()` returns a range — two schedules differing by less than the calibration's own width have not been shown to differ.

Deliberately one constant per drug. A single curve constrains one number; fitting four would give four that jointly fit and individually mean nothing, and the engine would absorb the data instead of being tested by it.

## The interval was wrong twice before it was right

Simulating curves with a **known** IC50 and counting how often the interval contains it (`scripts/validate_plate_fit.py`):

| Method | Coverage at nominal 95 % |
|---|:-:|
| resample raw residuals | **80 %** |
| scale by √(n/(n−p)) | 88–92 % |
| bias-corrected accelerated | **88–90 %**, no bias in the point estimate |

The first gap has a definite cause: a fitted curve's residuals are smaller than the true errors, because the fit absorbed p of the n degrees of freedom. The measured number is now printed next to every interval.

## What a plate has to look like for the number to mean anything

**Points per curve** — the result worth giving a biologist *before* they run the assay:

| Points | Coverage | Interval width |
|:-:|:-:|:-:|
| **5** | **68 %** | 1.85× |
| 7 | 86 % | 1.83× |
| 10 | 88 % | 1.75× |

Five points fit four parameters to five numbers. The interval comes back no wider than a ten-point one and is wrong a third of the time, so `fit_4pl` attaches an explicit note below seven points and quotes 68 % there rather than 90 %.

**Range** — a range that brackets the IC50 behaves (86 %); one that sits entirely to one side is **refused** (a flat plate has no midpoint); one that merely *clips* the response gives an interval of **121–168×** that still misses the truth a third to a half of the time. A wide interval there is not a conservative answer, it is an absent one — the `extrapolated` flag, not the width, is what to act on.

## Bugs these found

- Letting the bisection widen freely meant it could reach *any* measured IC50 — asked for 1e-9 µM it returned a paclitaxel partition coefficient of **six million**. Now bounded at three decades, with a refusal.
- The reported "calibrated engine gives X" was the previous bisection step, describing a constant the caller never receives.
- Grid exports pad unused wells, and `empty` was classified as a **blank** — 42 zero-reading fillers averaged into the background, silently rescaling every viability on the plate.
- Readers write `B07` where plate maps write `B7`; unnormalised, every well fails to match.
- `np.trapezoid` is NumPy ≥ 2.0 only; the pinned env has `np.trapz`.

## Performance

Calibration recomputed a whole IC50 curve each iteration to answer a yes/no question. The simulated IC50 equals T exactly when viability at T is ½, so bisecting on that single viability asks the same question with one simulation instead of ~30: **118 s → 20 s** per calibration, test file 6:44 → 2:51.

## Tests

`tests/cell/test_plate_smoke.py` (11 gates, 9 s) builds real plate files on disk in both layouts. `tests/cell/test_calibrate_smoke.py` (8 gates, 2:51) — its strongest case needs no external truth: perturb a drug's library constant, ask the engine what IC50 that produces, hand only the IC50 back, and check the calibrator recovers the constant it was never told.

Full cell suite: 0 failures.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e323a9471b02`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
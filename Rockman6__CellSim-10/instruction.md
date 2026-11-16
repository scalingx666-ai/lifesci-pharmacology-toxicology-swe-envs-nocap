# Issue

**Title:** Fix all 13 BUG_AUDIT findings in the FEP binding pathway (Milestone B)

## Description

Fixes every finding in `BUG_AUDIT.md` (2026-05-31, 13 confirmed: 7 HIGH / 3 MEDIUM / 3 LOW). None had been fixed before this branch — the post-audit commits only added replica exchange, which is orthogonal to the sign/cycle/FF bugs.

## Verification
- `tests/fep` suite: **137 passed** (baseline was 110; every fix ships a regression test).
- `tests/bridge tests/chem tests/uq`: **13 passed** (no cross-module breakage).
- End-to-end sampled DDM cycle runs to completion; methane/1ubq (a non-binder) now gives a correctly *unfavorable* ΔG_bind (~0 to +) — before the sign fix it read as a false *favorable* binder.

## Findings → commits
| Finding | Commit | Fix |
|---|---|---|
| #1/#3 standard-state sign | `db1e79a` | Correction added the *release* value (−) instead of the confinement work (+); ~10.5 kcal/mol over-binding bias. Pinned against openmmtools' own `HarmonicRestraintForce` SSC. |
| #2 seed, #8 n_windows | `ed33bd0` | n_windows<3 crash → clear guard. Seed plumbed to the integrator (see caveat below). |
| #5 FF mismatch | `cb52171` | Solvent leg now mirrors the complex leg's water model + 1.0 nm PME cutoff so the bulk-water term cancels in the DDM subtraction. |
| #6 stereo | `1812032` | `allow_undefined_stereo=True` on the solvent/hydration legs (was an asymmetric mid-run crash). |
| #7/#9/#10/#11 restraint geometry + r0 | `1047e15` | Restraint rest length set to the placement→anchor distance (was ~88 kcal/mol of misplacement on the test case); standard-state correction made r0-aware via an exact erf closed form (verified vs numerical integration). |
| #12/#13 hygiene | `737f1b7` | Sign-inverted docstring on `estimate_free_energy`; replica-exchange temp-dir leak. |
| #4 restraint-on-real leg | `eded459` | Was silently zero. Added an opt-in sampled leg (`--restraint-on-real-leg`); when off, the result is explicitly flagged provisional (no longer silent). ΔΔG cancels it. |

## Self-review (max effort) found and fixed 2 defects in the fixes
`ecbb196` + streptavidin-runner commit:
1. **Seed reproducibility (#2) was overclaimed.** Seeding `_get_integrator` is a prerequisite but *not sufficient* — openmmtools' `ContextCache` steps a separately-managed integrator. Confirmed empirically: `seed=1` twice differs on **both** the deterministic Reference and OpenCL platforms. Claim, docstrings, and test corrected to be honest; true reproducibility needs a direct-Context sampler refactor (tracked follow-up). Interim guidance: report FEP as mean ± SD over seeds.
2. **The provisional (#4) note was written to `reason` on an `ok=True` result**, making it invisible in `summary()` and misleading to CSV consumers. Moved to the `restraint_on_real_included` flag + `summary()`.

## Status / merge intent
- **ΔΔG ranking is trustworthy now** (the corrections cancel in B−A) — that's what Milestone B gates on.
- **Absolute ΔG_bind is correct in construction but not yet signed off**: needs the GPU biotin/streptavidin gate (`|ΔG_bind − (−18.3)| < 3`, `--replica-exchange --restraint-on-real-leg`, both wired into `scripts/run_binding_streptavidin_gpu.sh`). The buggy code landed near −29.
- **12 of 13 findings fully fixed + tested; #2 (seed) is honestly scoped** as plumbing-landed / reproducibility-not-achieved with a documented path forward.

The prior untracked `run/fep/streptavidin_*` result predates these fixes and should be re-run on this branch before any absolute number is reported.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aecd9f1f0ac1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
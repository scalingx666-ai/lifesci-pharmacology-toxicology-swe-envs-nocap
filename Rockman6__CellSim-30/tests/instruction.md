# Issue

**Title:** Necrotic core: a solver bug in where the medium began

## Description

The necrotic-core miss was a **solver bug**, not biology.

## The symptom

Grimes's anoxic-core relation returned a diffusion limit of **173 µm** against a measured 233 ± 22 — and it **drifted down daily** (196 → 157 µm), though a diffusion limit is a fixed property.

## Three hypotheses, all rejected

| Hypothesis | Verdict |
|---|---|
| Formation overshoot *(was in the docs)* | dead core sits **inside** the anoxic region |
| Surface roughening *(was in the docs)* | surface gets **smoother** as it grows |
| Quiescent cells consume less | shifts the level, leaves the drift |

## The actual cause

The engine's own constants give an analytic limit of **exactly 233 µm** — the parameters were right, the solve wasn't delivering them.

The medium's oxygen was applied at the **single outermost occupied cell**. Stray cells drifted out, putting the medium **159 µm beyond the spheroid** by day 12. All its oxygen had to diffuse through that much empty fluid; surface O₂ fell 88 → 32 mmHg. The layer thickened with growth — which *is* the drift.

## Fix

Medium now begins at the **equivalent-sphere radius** — the same R Grimes's relation uses. Chosen for consistency, not tuned.

| | r_l | Drift | Surface O₂ |
|---|:-:|:-:|:-:|
| old | 173 µm | −6.5 µm/day | 88 → 32 |
| **new** | **240 µm** | **+1.9 µm/day** | **94 → 93** |
| analytic | 233 µm | — | — |

**Strongest evidence:** oxygen physics stopped depending on mechanics. Across `push_sites` 1/2/3/∞: before **172–219 µm** (47 µm spread), after **240–248 µm** (8 µm spread).

## Cost

`push_sites = 1` was calibrated *under the bug* — two errors partly cancelling, which is how it survived. Growth is now 17.1 µm/day vs ~15 (was 15.3), ~14 % fast; reach 1 is still the best tried.

Full suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ccdfccf19cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
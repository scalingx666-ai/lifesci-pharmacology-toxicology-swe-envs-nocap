# Issue

**Title:** Four new experiments: gene knockouts, glucose starvation, cell migration, immune killing

## Description

Four capabilities attempted rather than scoped out. All default to **off**, and the engine is verified **bit-identical** across its reference outputs when they are unused.

## Gene knockouts — `api.knockout(line, drug, gene)`

13 genes mapped to engine nodes; knockout, knockdown or overexpression. A gene the engine does not represent is **refused**, because a knockout that silently does nothing looks like a result.

| Check | Result |
|---|---|
| TP53 knockout vs the p53-mutant lines | lands in the same place |
| PUMA vs BAX vs CASP3 knockout | identical — one linear pathway |
| ABCB1 raised, vs doxorubicin | protects (a P-gp substrate) |
| ABCB1 raised, vs **cisplatin** | **no change** — not transported |

The last pair is the real test: a framework that moves everything is not modelling anything.

Prediction worth recording: **blocking apoptosis converts killing into arrest** — BAX-null viability floors at 0.14 instead of reaching 0.

*Bug found:* the first BAX knockout had no effect. The constraint was applied partway through `rhs`, and later blocks assign their slice of `dY` outright, overwriting it. Now applied last.

## Glucose — `api.starve(line, glucose_mM=...)`

A second nutrient on the same reaction-diffusion solver as oxygen; whichever is scarcer governs, since Freyer & Sutherland (1986) showed a spheroid's core can be set by glucose rather than oxygen.

| Medium glucose | Live | Necrotic | Radius |
|---|:-:|:-:|:-:|
| 5.5 mM | 10 840 | 105 | 207 µm |
| 0.2 mM | 6 903 | 0 | 177 µm |
| 0.1 mM | 3 488 | 615 | 149 µm |

Growth stalls before killing begins, which is the right ordering. Literature thresholds — **calibrated, not validated**.

## Migration — `DishParams.migration_*`

A random walk on the lattice, optionally up the oxygen gradient. This one has an exact answer to check: D = k·h²/4.

| Hop rate | Simulated D | Theory |
|---|:-:|:-:|
| 0.5 /h | 43 µm²/h | 50 |
| 2.0 /h | 198 µm²/h | 200 |

*Bug found:* one hop per field interval capped the effective rate, giving 126 against a theoretical 200. Now runs in sub-rounds.

## Immune killing — `api.killing(line, et_ratios=...)`

Effector cells on their own layer that crawl, engage, kill, rest while conjugated and stop after a few kills (Halle et al. 2016).

| E:T | Live tumour | Specific lysis | Kills |
|---|:-:|:-:|:-:|
| 0 | 862 | — | 0 |
| 0.1 | 546 | 37 % | 192 |
| 0.3 | 100 | 88 % | 458 |
| 1.0 | 0 | 100 % | 446 |

Kill counts *fall* above E:T 0.3 as targets run out — the right shape. Literature kinetics — **calibrated, not validated**. The gates pin that the lysis comes from the killing and not from the effectors' presence: a zero kill rate leaves the monolayer untouched.

## Also

`pip install cellsim` in the README, now that the package is published and verified from a clean venv.

## Tests

`test_perturb_smoke.py` 9/9 (43 s), `test_tissue_env_smoke.py` 10/10. Full cell suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `34fc9fc14dbb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
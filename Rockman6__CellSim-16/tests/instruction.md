# Issue

**Title:** Phase 2: two-drug combinations — antagonism reproduced, sequence effect shown to be an artefact

## Description

Completes the Phase 2 target list. Every result here is **within-line** — same drugs, same doses, same cell line, only the timing differs — so none of it is limited by the per-line prediction ceiling measured in Phase 1.

## Engine: real combinations

One intracellular concentration slot per drug, so `simulate(line, [A, B], [c_a, c_b], ...)` is a genuine two-drug run rather than two single-drug runs compared. A single drug or `None` keeps the previous state layout exactly; the full cell suite (28 files) is unchanged and passing.

## Antagonism — reproduced, and gated

Cisplatin damages DNA → p53 → p21 → cycle arrest. Paclitaxel kills only cells that attempt mitosis while it is present. So the platinum removes the very cells the taxane needs:

| Arm | surviving | vs independent |
|---|:-:|:-:|
| cisplatin alone | 0.598 | |
| paclitaxel alone | 0.409 | |
| **independent expectation** | **0.244** | — |
| both together | 0.323 | **1.32× antagonistic** |
| cisplatin → paclitaxel | 0.341 | 1.40× |
| paclitaxel → cisplatin | 0.366 | 1.50× |

Nothing was fitted. The arrest is the p53 → p21 axis ported from the C++ prototype; the mitosis requirement is the tubulin occupancy threshold.

## The sequence effect is an artefact — the more useful finding

I predicted taxane-first would win. The engine said platinum-first, by a small margin, so I checked whether it survives a longer readout:

| readout | cis → pac | pac → cis | ratio |
|---|:-:|:-:|:-:|
| 72 h | 0.341 | 0.366 | **0.93** |
| 96 h | 0.345 | 0.351 | 0.98 |
| 120 h | 0.347 | 0.353 | 0.99 |

Cisplatin acts slowly — adducts, then p53, then commitment — so placing it **second truncates its effect inside a fixed window**. The apparent ordering advantage is a confound between *when the drug was given* and *how long it had left to act*.

So: the engine reproduces the antagonism and **does not** reproduce a biological platinum/taxane sequence dependence. What it does instead is expose a trap that fixed-endpoint combination assays fall into. The tests pin the antagonism and pin the artefact, and deliberately **do not** assert a sequence effect — pinning that would be pinning an artefact.

## Phase 2 status

| Target | State |
|---|---|
| Schedule dependence | done — interior optimum for paclitaxel, published exposure-time ordering |
| Wash-out / recovery | done — 6 h pulse regrows ×4.0 where continuous dosing eradicates |
| Selection under treatment | done — dose intensity decides *which axis* of resistance is selected |
| Combination timing | **this PR** |

3 new gates, 22 s. Numbers and reasoning in `docs/VALIDATION.md`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f39b1f558b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
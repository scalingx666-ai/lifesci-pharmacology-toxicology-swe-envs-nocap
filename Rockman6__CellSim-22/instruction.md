# Issue

**Title:** Phase 5: the proliferation/quiescence split — and the parameter conflation it exposed

## Description

## What this adds

A daughter does not always start the next cycle: CDK2 activity bifurcates at mitotic exit and the low branch sits in G0 until recruited (Spencer et al. 2013 Cell 155:369). The engine had no such state — every cell cycled — which is the documented miss against the Cell Tracking Challenge HeLa movie, where 40–60 % of tracked cells never divide inside 46 h and the Kaplan–Meier curve **plateaus** while the engine's keeps falling.

`Params.quiescent_fraction` and `quiescence_exit_per_h`, both **0 by default** and verified bit-identical when off.

## It took two changes, and the second is the finding

| | 20 h | 30 h | cells at 46 h |
|---|:-:|:-:|:-:|
| measured, seq 01 | 0.77 | 0.51 | 137 |
| engine, 31 h cycle, no quiescence | 1.00 | 0.52 | 126 |
| **engine, 19 h cycle + 50 % quiescent** | **0.74** | **0.51** | **140** |
| measured, seq 02 | 0.80 | 0.71 | 363 |
| engine, 31 h cycle, no quiescence | 0.94 | 0.38 | 385 |
| **engine, 19 h cycle + 50 % quiescent** | **0.72** | **0.63** | **364** |

Quiescence alone fixed the 30 h plateau and left the engine far too slow at 20 h. The cycling cells also had to cycle faster — and that exposed:

**`doubling_time_h` is a single-cell cycle time, and the library fills it with population doubling times.** `calibrate_cycle_scale` times one cell from early G1 to mitosis; ATCC and Cellosaurus quote how long a culture takes to double. They agree only when every cell cycles. HeLa cycles in ~19 h while its counts double every 27–31 h, so the library's 31 h makes population growth right and every single-cell cycle 1.6× too slow.

It went unnoticed because every validation to date was population-level, where the conflation is invisible. It took data tracking *individual cells over time* to surface it.

## HeLa's entry is deliberately NOT changed

Splitting the parameter means re-validating the GDSC predictions resting on it. That cost is now measured rather than guessed (A549, half of each generation quiescent):

| Drug | Mechanism | IC50 shift |
|---|---|:-:|
| gemcitabine | S-phase | **1.50×** |
| sn-38 | TOP1, S-phase | **1.26×** |
| cisplatin | DNA adduct | 1.06× |
| doxorubicin | TopII | 1.03× |
| paclitaxel | tubulin | 0.95× |

So the change would move antimetabolite predictions by about a third and leave the rest alone.

**One prediction refuted.** I expected the taxane to lose potency too — a quiescent cell never reaches mitosis. It didn't. The arms show why: under gemcitabine the treated colony shrinks *less* than the control (×0.57 vs ×0.46), because quiescent cells escape a drug that **kills** and are enriched by it; under paclitaxel both shrink together (×0.44 vs ×0.46), because a cell that wasn't going to divide loses nothing when division is blocked. Protection comes from escaping killing, not from escaping cytostasis.

## Tests

`tests/cell/test_quiescence_smoke.py` (5 gates, 10 s) pins the mechanism rather than the fit: off by default, a genuine hold rather than a slow-down, reversible on recruitment, and not a form of death. Full cell suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e281d0cb483`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
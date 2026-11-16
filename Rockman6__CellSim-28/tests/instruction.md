# Issue

**Title:** Engine: paclitaxel 24 h miss from 240x to 3.4x

## Description

Fixes the engine's worst documented defect: a 24 h paclitaxel exposure was **240× too potent** against Georgiadis 1997. Now **3.4×**.

## Two causes

**1. Drug left as fast as it entered.** Uptake was symmetric, so wash-out emptied a cell in minutes. Paclitaxel is held on tubulin and shows little efflux after removal. New `Drug.tau_efflux_h` (24 h for taxanes; 0 = symmetric, so no other drug changes).

Without it the 24 h curve had an **unreachable floor** — survival stuck at 0.42 from 0.1 to 30 µM.

**2. The mitotic death rate was never determined by the fit.** Fitted to 72 h GDSC screens, but a 72 h IC50 moves under 10 % while that constant varies 100-fold. Short exposures *do* constrain it.

| | before | after | measured |
|---|:-:|:-:|:-:|
| 3 h | 73 µM | 147 µM | > 32 ✓ |
| 24 h | 0.039 µM | **2.7 µM** | 9.4 |
| continuous | 0.028 µM | 0.027 µM | 0.027 ✓ |

GDSC panel unchanged (paclitaxel median fold error 2.4× before and after).

**Honest reading:** the 24 h point is now calibrated rather than predicted. The 3 h point stays independent and passes. Docetaxel and vinorelbine keep the old value — no short-exposure data for them.

## Three tests needed three different calls

- **3 h "unreachable"** → now finite at ~150 µM, which matches the measurement ("above 32", not "impossible") better
- **AUC threshold** → shape intact; only a boundary set by the old death rate missed by 0.027. Now checks shape
- **Antagonism** → intact at 1.26× over five seeds; was single-seed and one landed at 1.095. Now seed-averaged

Two of my diagnoses along the way were wrong and are recorded in the commit.

Full cell suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d6f9ed0ac0b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Immune killing: the measured three-hit mechanism, validated

## Description

The first immune-killing model used the wrong mechanism: each contact killed outright. Weigelin et al. 2021 (Nat Commun 12:5217) showed with live imaging that a single contact rarely kills — each hit is sublethal and repaired, and death needs about **three hits within ~50 min**, usually from several effectors.

Now implemented. `effector_hits_to_kill = 1` keeps the old picture for comparison.

## What emerged, unfitted

| E:T | Lysis | Hits per kill |
|---|:-:|:-:|
| 0.1 | 0 % | 418 |
| 1.0 | 40 % | 17 |
| 3.0 | 100 % | 4.1 |

Sparse hits are repaired before a third lands, so killing needs density — which is why CTLs swarm.

## Mechanism checks (`scripts/validate_killing.py`) — all pass

1. Hits per kill falls steeply with density
2. Repair window 15 / 30 / 50 / 120 min → lysis 6 / 18 / 40 / 96 %
3. One-hit killing recovers the old efficiency (39 % vs 0 % at E:T 0.1)
4. No kill without three hits

**Not validated:** absolute lysis against a measured killing curve. None digitised; rates stay literature values.

## Gates

Two gates encoded the old mechanism's numbers and failed when it changed. Moved to E:T 3, and the exhaustion gate now asserts killing happens before comparing caps.

Full cell suite: 0 failures.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b42bd11c7db5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
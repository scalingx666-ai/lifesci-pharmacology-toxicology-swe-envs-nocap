# Issue

**Title:** Glucose acts through respiration, not starvation — now validated

## Description

Glucose failed its validation (PR #26): byte-identical answers across the whole published 16.5–0.8 mM range. **It modelled the wrong mechanism.**

## What was wrong

Glucose was made to kill by **starvation**. At reachable spheroid sizes the centre never fell below the threshold — not one cell of 29 685 was starving.

## What the papers actually describe

Alongside the thinner rim, the same work reports **respiration rising as glucose falls**. Casciari et al. 1992 measured oxygen consumption nearly doubling between 5.5 and 0.4 mM. Starved cells burn more oxygen → the spheroid goes hypoxic sooner → the rim thins, with no cell starving. **The effect runs through oxygen.**

## Result

| Glucose | Rim, 20 % O₂ | Rim, 5 % O₂ |
|---|:-:|:-:|
| 16.5 mM | 175 µm | 58 µm |
| 5.5 mM | 174 µm | 58 µm |
| 1.65 mM | 135 µm | 46 µm |
| 0.8 mM | **111 µm** | **39 µm** |

All five directional claims pass, within the published range, at both oxygen levels.

## Why it's a test, not a fit

The coupling's anchors come from **Casciari 1992**; the claims it's checked against come from **Mueller-Klieser 1986** — different papers. Only the curve shape between anchors is free, and it's labelled.

**Not checked:** magnitudes. The published rim widths are image-only tables.

## Other changes

- Glucose solved before oxygen, so uptake can depend on it
- One gate relaxed from exact equality to <2 % — honest difference is 0.2 %, from the continuous respiration curve
- Removed the script's claim that respiration compensation was missing, which had stopped being true

Glucose stays **off by default** — no earlier result moves. Full suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79b4d88629c9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
# Issue

**Title:** Glucose fails its validation, and is labelled so

## Description

`scripts/validate_glucose.py` tests the directional claims of Mueller-Klieser, Freyer & Sutherland (1986). **Two of five fail.**

| Medium glucose | Live | Radius | Viable rim | Necrotic core |
|---|:-:|:-:|:-:|:-:|
| 16.5 mM | 42 309 | 337 µm | 175 µm | 162 µm |
| 0.8 mM | 42 309 | 337 µm | 175 µm | 162 µm |

Identical across the whole published range. Oxygen claims pass; glucose claims do not.

**Cause:** glucose penetrates ~260 µm at 0.8 mM, so at a 311 µm radius the centre reaches 0.470 mM against a quiescence threshold of 0.30 — not one cell of 29 685 is below it. Their spheroids were ~1000 µm across.

**The feature stays, labelled as failing.** One of consumption, thresholds or comparison is wrong, and the data to say which isn't in hand. Tuning until it passes would be fitting to the answer.

**A first version of this test reported 5/5 passing** — by reaching down to 0.1 mM, below anything measured, on spheroids too small for a gradient to form. A test that leaves the experimental range to find an effect is manufacturing a pass. README no longer claims the feature works.

Also recorded: the missing respiration compensation (cells burn more O₂ as glucose falls).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b42bd11c7db5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
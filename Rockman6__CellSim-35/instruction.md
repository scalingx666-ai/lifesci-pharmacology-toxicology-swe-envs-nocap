# Issue

**Title:** Inside a cell: watch every molecule the engine tracks

## Description

Adds a second Lab view, **Inside a cell**: a signalling map where every node's brightness is that molecule's real level from the engine, minute by minute — drug arriving, DNA damage, ATM/p53, the cyclins driving division, and the death cascade from PUMA to caspase-3. Six cells per run, with play / scrub / speed.

`cellsim/cell/trace.py` records it through `simulate`'s own `on_record` hook — nothing is computed differently from the engine.

**Honesty fixes found by looking:** brightness first scaled each species to its own run maximum, so a healthy cell's resting p53 glowed as bright as nutlin's and the text said *p53 has risen*. Now fixed scales, drawn as rise above rest.

**Two engine defects it revealed** (every earlier check looked only at 72 h):
- nutlin ~5–10× too fast — cells die in 5 h, not 1–2 days
- cisplatin barely raises p53 (<2-fold)

Not patched without measured time courses; the page shows a *Known limitation* box on both, and `VALIDATION.md` records them.

Tests: `test_trace_smoke.py` 5/5.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5d603ca42e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
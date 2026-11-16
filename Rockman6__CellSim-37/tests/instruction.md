# Issue

**Title:** Inside a cell: look up any drug, or design your own (1.6.0)

## Description

## What

**Inside a cell** now takes any drug by name or structure, and lets you design your own.

**Look-up** (`web/lab/lookup.js` + `cellsim/cell/compound.py`): PubChem for structure and properties, ChEMBL for the curated mechanism (queried by parent molecule; platinum complexes found by name), RDKit's browser build for drawing and for molecules nobody has made.

| Typed | What happens |
|---|---|
| a library drug (cisplatin) | uses the library entry, calibrated |
| a modelled mechanism (topotecan → TOP1) | runs with potency borrowed from a library drug of the class, labelled *Not calibrated*, ×0.01–×100 strength |
| an unmodelled target (gefitinib → EGFR) | refused by name, with "Design what it does →" |
| an impossible structure | rejected: "Atom 2 (C) would have 6 bonds — more than C can form" |
| a valid molecule nobody has recorded | properties computed, no target claimed, design offered |

**Design mode**: block or boost any molecule on the map (`Drug.mechanism = "target"`), or reuse a library mechanism at a chosen strength. Labelled *Your hypothesis*.

**Fixes**: the drug's arrow on the map pointed at DNA damage for every drug (nutlin, paclitaxel included); it now goes where the drug acts, routed between zones. A knockout combined with a two-drug combination crashed (`operands could not be broadcast together with shapes (8,22) (1,21)` on the previous engine).

## Tests

- `tests/cell/test_design_smoke.py`: a saturating blocker equals the gene knockout and a booster the overexpression (`cellsim.cell.perturb`), dose = Kd blocks partway, knockout + combination runs, refusals.
- `tests/cell/test_compound_smoke.py`: each library drug's own ChEMBL mechanism maps back to its library mechanism; EGFR/PARP/BCL-2 refused.
- Full suite at `CELLSIM_TESTS=all`: 41/41 files pass locally.
- Driven in headless Chrome: topotecan, gefitinib, an impossible SMILES, cisplatin, an invented molecule, two designs — no page errors.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ec9cce3261ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
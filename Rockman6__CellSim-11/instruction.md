# Issue

**Title:** Fix scrambled-molecule docking bug + populate Layer 1.7 blind validation

## Description

## The bug (load-bearing)

The docking layer assumed Meeko "preserves SMILES atom order." It does **not** — Meeko reorders atoms during ligand prep (biotin: SMILES `OCOCCCCC…` → PDBQT `CCCOOCCC…`). Both consumers that need a correct 3D molecule — `_build_pose_mol` (PoseBusters) and `pose_rmsd_symmetry_aware` (crystal RMSD) — mapped pose coordinates onto a `MolFromSmiles` template **by index**, producing a topologically scrambled molecule with physically impossible 3–5 Å "bonds" (12/17 wrong on biotin).

**Impact:** this silently corrupted *both* headline numbers of Layer 1.3a — pose recovery and PoseBusters validity. Biotin/1STP was reported at 2.06 Å (borderline fail) when the real redock is **0.69 Å** (excellent). Good poses were scored as bad.

## The fixes (5 commits)

1. **`2d222e4`** — reconstruct poses via Meeko's reverse conversion (`PDBQTMolecule` + `RDKitMolCreate.from_pdbqt_mol`), storing a correct mol block on each pose. 41/41 dock tests pass; regression test guards against the scramble returning.
2. **`1e9e845`** — new blind-validation harness (`scripts/run_blind_dock_bench.py`): fetches structures **and authoritative ligand SMILES from RCSB at run time** (not from memory — so it's not circular on our inputs), auto-picks the drug ligand, re-docks blind, scores RMSD + PoseBusters. Populates the previously-empty Layer 1.7.
3. **`7e52547`** — robust crystal-RMSD: one-copy extraction (multi-copy crystals) + MCS common-core matching (off-by-one atom counts) + ring-info init. All 15/15 blind entries now score.
4. **`0c8f265`** — centre the docking box on one ligand copy (multi-copy crystals were centred between binding sites → docked into empty space). 2hyy 2.77 → 0.80 Å.
5. **`docs`** — correct the README/BENCHMARKS/status numbers that were computed on scrambled molecules.

## Honest Layer-1.7 results (15 diverse cocrystals)

| Criterion | Result | Gate | Verdict |
|---|---|---|---|
| #3 PoseBusters physical validity | **100%** | ≥95% | ✅ PASS |
| #1 Pose recovery top-1 ≤2 Å | **73%** (11/15) | ≥75% | ⚠️ just under |
| Pose recovery top-3 ≤2 Å | **87%** (13/15) | — | (sampling) |

The top-1/top-3 gap is the finding: Vina's **sampling** finds the correct pose 87% of the time; its **scoring** ranks it #1 only 73%. A UFF-strain re-rank does not recover the ranking failures. This is the same limitation as criterion #2 (kinase ranking) — Vina scoring — which is exactly what FEP replaces.

`tests/dock`: 43 passed. Separate from the FEP audit branch (PR #10).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aecd9f1f0ac1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
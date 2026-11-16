# Issue

**Title:** fix(data): skip SMILES that RDKit parses to None in filter_invalid_smiles

## Description

For addressing #10 

filter_invalid_smiles called mol.GetNumHeavyAtoms() directly on the result of Chem.MolFromSmiles, which raises AttributeError when RDKit returns None for an unparseable or chemically-invalid SMILES. RDKit version differences make this common: a string that parsed under an older RDKit can return None after an upgrade (e.g. stricter valence handling), so a dataset that loaded before now crashes at the filtering gate that is supposed to drop it.

Add a None guard between MolFromSmiles and the heavy-atom check, matching the function's existing print-and-continue style. Reported externally.

Adds tests/unit/test_data_utils.py covering the previously-crashing path (unparseable + valence-invalid SMILES), all-valid passthrough, and the zero-heavy-atom case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e402473376ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
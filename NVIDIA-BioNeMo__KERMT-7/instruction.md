# Issue

**Title:** Bump cuik-molmaker to v0.2

## Description

### Bump `cuik-molmaker` to v0.2

- Bump cuik-molmaker from 0.1.1 to 0.2 and switch installation from pip (NVIDIA PyPI) to conda (conda-forge)
- Update all call sites to use the new cuik-molmaker 0.2 API: *_to_tensor() functions renamed to *_to_array(), which now return NumPy arrays instead of Torch tensors
- Add torch.from_numpy().float() conversions where cuik-molmaker outputs feed into the PyTorch model, ensuring float32 dtype consistency
- Update rdkit dependency from 2025.03.2 to 2025.09.1 to match cuik-molmaker 0.2 requirements

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1eed45682e78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
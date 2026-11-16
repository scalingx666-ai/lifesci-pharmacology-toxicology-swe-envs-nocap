# Issue

**Title:** HW-1: Potential Edit for chemextensions function PCAAnalysis()

## Description

**Suggested Edit (should be generally helpful)**
Noticed that the default code appeared to plot PC1 and PC3, so updated to plot PC1 (`chemicalspace[:,0]`) and PC2 (`chemicalspace[:,1]`) as collectively, these should explain a higher proportion of the variance (versus just PC1 + PC3 (`chemicalspace[:,2])`).

**Suggested options (these were helpful to us; might be helpful for others)**
- We wanted to overlay the PC output with our own experimental data, so needed to export the PCs. 
-  Added code (using `pandas`) to output PCs to TSV. (with user option **save_pcs**).
- As a note: by outputting the first 3 PCs, we were able to see clearer separation when in a three dimensional plot.
- Updated so that the user-provided filename would be the same base name for the TSV and HTML. 
- (note: did not yet update `cheminformatics.py` with save_pcs option )

(Enjoyed using PCAAnalysis() and was able to successfully analyze a provided list of SMILES Ids! Found via Medium post. Thanks!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5b5a0f7dfea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
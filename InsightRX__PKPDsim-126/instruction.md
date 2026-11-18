# Issue

**Title:** fix thomson and install function

## Description

- Thomson has 3 fixed parameters, should be only 1. This throws an error in get_map_estimates() currently. Since we remove TDM_INIT, we also need to switch this model to not use an init parameter. Also fixed this same issue for two other models.

- The function install_default_litreature_model() is not passing … along to model_from_api(). It says in documentation it should (and I also think it should)

None of this should affect any production code, this is all example code / models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ddc6f66b6c3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
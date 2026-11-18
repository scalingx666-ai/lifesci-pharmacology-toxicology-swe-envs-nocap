# Issue

**Title:** Add compartments cmt to regimens in nm_to_regimen

## Description

This PR changes nm_to_regimen so that it uses a map for compartment to dose type. This is useful for mipdeval, where this is needed.

This breaks backward compatibility because we now want an explicit declaration of the type of dosing instead of inferring from presence/absence of columns.

This function is used in two places:

1. PKPDposterior::nonmem_to_stan_data:40, which we haven't touched in a while
2. mipdeval::parse_input_data:45, which is where we have the temporary fix

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98131d2728de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
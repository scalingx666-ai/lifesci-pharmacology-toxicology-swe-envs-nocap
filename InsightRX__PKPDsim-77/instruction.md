# Issue

**Title:** use LOCF when non-numeric too; check for implementation type

## Description

There was a unique situation that was not being handled correctly: 

- if the value supplied was non-numeric and 
- the `remove_negative_times` argument was `TRUE` (default), and 
- there were negative times, with the non-numeric value changing on either side of `t = 0`

then we were not actually carrying the last observation forward, but instead filtering the observations.

This is a pretty niche situation (`PKPDsim::sim`, the intended consumer of this data object requires numeric values) but when these values are used as placeholders for later use it was leading to some confusing results.

For safety, I also added a check to ensure the implementation method specified matches what we say in the documentation are the acceptable input types.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5bbc5d686df0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
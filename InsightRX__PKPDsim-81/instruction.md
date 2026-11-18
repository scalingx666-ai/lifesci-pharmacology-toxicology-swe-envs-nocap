# Issue

**Title:** rm assumption non-inf types are t_inf = o

## Description

In create_event_table, two changes were necessary to enable simultaneous infusion-type doses in different compartments.

1) needed to assume that infusion doses could have a "type" other than "infusion". Changed logic to reference `t_inf`, assuming the user specified this correctly as non-zero.
2) needed to fix logic to also consider dose cmt when looking for duplicated rows

Added a test too

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7134bc1ea560`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
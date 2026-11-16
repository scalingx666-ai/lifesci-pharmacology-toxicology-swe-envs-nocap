# Issue

**Title:** Add co-located evals for the 8 agent skills

## Description

## What
Adds `agent/skills/<skill>/evals/evals.json` for all 8 KERMT skills.

## Why
These eval definitions currently live **only** in the `bionemo-agent-toolkit`
aggregator catalog (added there directly via that repo's PR #4,
`evax/add-kermt-evals`). KERMT is a **sourced** skill in the aggregator, so the
nightly sync mirrors its catalog dir from *this* repo with `rsync --delete` —
which deletes any catalog-only files. The evals would be lost on the first sync.

The fix, per the aggregator's "source of truth is upstream" model, is to put the
evals **here**. This also satisfies the aggregator compatibility requirement
**SRC-10** (co-located `evals/evals.json`), which makes these skills ready for
aggregation into `NVIDIA/skills` as well — not just bionemo-agent-toolkit.

## Provenance
Files copied verbatim from
`bionemo-agent-toolkit@main:open-models-skills/kermt/skills/<skill>/evals/evals.json`.
Please sanity-check they match current skill behavior.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `408b86344cb9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
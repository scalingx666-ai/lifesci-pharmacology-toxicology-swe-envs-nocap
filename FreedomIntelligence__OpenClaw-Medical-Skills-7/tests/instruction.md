# Issue

**Title:** Apply Minimal Sanity Check on SKILL.md via Github Action and Refactors the biomaster-workflow Skill

## Description

This PR refactors the biomaster-workflow skill and is related to #4 .

### Changes
- Remove codes containing api key
- Improve readibility
- add Github action that validate skill every time a SKILL.md file is added/modified (not tested yet)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d137f53603f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
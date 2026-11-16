# Issue

**Title:** chore: remove deprecated description validation logic

## Description

Hi maintainers,

Thank you again for your help earlier!

This is a tiny, low-risk cleanup PR to remove some deprecated "zombie code" from `scripts/validate_skill.py`.

### What's changed?
Removed several lines of commented-out validation logic that enforced strict character (`<`/`>`) and length limits on skill descriptions.

### Why it's safe to remove?
1.  **Outdated constraints**: These limits were from an earlier version of the plugin spec, before the migration to the new `@openclaw/plugin-sdk` architecture.
2.  **New SDK capabilities**: The current SDK and MCP framework already handle richer description formats (including Markdown) gracefully, making these rigid checks unnecessary.
3.  **Clean code benefit**: Removing commented-out dead code reduces cognitive load for future readers and maintainers. The full history is always available in Git if needed for reference.

This change does not affect any active functionality or validation logic.

Thanks!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b5398e2f7e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
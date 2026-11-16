# Issue

**Title:** feat: migrate to new plugin SDK

## Description

### Summary
This Pull Request covers two main updates:

**Migration & Upgrade**: Migrated the entire medical skills library from the deprecated OpenClaw extension API to the new `@openclaw/plugin-sdk` architecture. This includes updating the plugin manifest and refactoring necessary configurations for the new framework.

### Changes Included
- Added/Updated `openclaw.plugin.json` manifest for the new SDK.
- Adapted skill configurations for compatibility with the new architecture.
- Fixed broken/missing markdown links in the READMEs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8b0c427f6a53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
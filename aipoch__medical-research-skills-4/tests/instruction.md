# Issue

**Title:** Add phi-prompt-guard skill (runtime PHI guardrail for LLM prompts)

## Description

## Summary

Adds a new behavioral skill at `scientific-skills/Other/phi-prompt-guard/SKILL.md` that prevents Protected Health Information (PHI) from entering the LLM context during live agent conversations.

## Why this skill exists

Compliance controls in healthcare (HIPAA training, BAAs, DLP, audit logs) assume humans move data deliberately — query, export, email. LLM assistants break that assumption: when an engineer pastes a debugging row, a SQL result, or a "just take a look at this CSV" into the chat, PHI exits the regulated perimeter silently. There is no download dialog, no VPN prompt, no reviewer thread — and absent a BAA with the provider, the data crossing the API is itself a HIPAA violation. `phi-prompt-guard` is a seatbelt for that moment.

## Relationship to the existing `hipaa-compliance-auditor` skill

These are **complementary, not duplicative**. They share the HIPAA Safe Harbor 18-identifier taxonomy but sit at opposite points in the data-flow:

| Skill | Mode | Operates on | When it runs |
|---|---|---|---|
| `hipaa-compliance-auditor` (existing, Academic Writing) | Batch text de-identification (Python: spaCy / Presidio / regex) | Static files, CSVs, manuscript text already on disk | After data is collected, before publication |
| **`phi-prompt-guard`** (this PR, Other) | In-context behavioral guard for the agent | The live prompt, tool output, and data the agent is about to fetch | Continuously, during the conversation |

Use the auditor to clean a corpus. Use this skill to keep PHI from ever entering the agent's context. No functional overlap. Confirmed via grep across all 414 scientific-skills SKILL.md files — `hipaa-compliance-auditor` is the only other PHI/HIPAA skill.

## Conformance with the repo's canonical SKILL.md pattern

Verified against the repo pattern observed across 414 SKILL.md files and the auditor's basic-evaluation rubric:

| Check | Result |
|---|---|
| Frontmatter fields (`name`, `description`, `license`, `author`) | ✓ All present, `license: MIT`, `author: AIPOCH` |
| `> **Source**:` line | ✓ Byte-identical to repo standard |
| Section order: `When to Use → Key Features → Dependencies → Example Usage → Implementation Details → Instructions` | ✓ Matches canonical |
| `kebab-case` skill name | ✓ |
| No name collision in `scientific-skills/Other/` | ✓ (`ls | grep -iE 'phi|hipaa|guard|safety|privacy'` returns nothing) |
| Synthetic-only examples (no real PHI in the SKILL.md itself) | ✓ Clinical-row example annotated `(synthetic — illustrative only)` |

External attribution is via a `> **Contributed by**:` line under the Source link (mirrors the existing Source-line pattern). I tried `author: "[@ndu-bioinfo](...)"` in the frontmatter first but reverted after an internal review — it was the only outlier across 414 files, and the repo README frames the library as developed in-house, so a frontmatter-level deviation would be confusing. Happy to reshape if maintainers prefer a different attribution scheme; there is no `CONTRIBUTING.md` to follow.

## Skill-auditor pre-check — PASS on all four veto gates

Ran `skill-auditor/scripts/evaluate_skill.py` against the new skill. All Step 1 veto gates pass:

| Veto gate | Result |
|---|---|
| T1. Stability | PASS |
| T2. Contract | PASS |
| T3. Determinism | PASS |
| T4. Security | PASS |

Classification: `Other`, execution mode `C (API)`, complexity `Moderate`. Output saved as `phi-prompt-guard_structural_precheck.json` in the skill directory.

**Full skill-auditor v2 evaluation deferred to maintainers.** The dynamic-execution score, category-specialized rubric, assertion results, and the final grade in `phi-prompt-guard_audit_result_v2.json` require live evaluation that should be produced by the repo's own evaluation workflow rather than hand-written. Happy to re-run if there is a documented contributor procedure for generating the v2 audit.

## Skill-only contribution (no hooks)

The upstream `phi-guard-skill` project also ships Claude Code hooks (`UserPromptSubmit`, `PreToolUse:Bash`) for hard, deterministic enforcement at the prompt boundary. Those are Claude-Code-specific and would not load under OpenClaw / Hermes — the other agent frameworks this library explicitly targets. To stay portable, this PR contributes **only the SKILL.md** (pure in-context behavior, no hooks, no installer, no plugin manifest). The hook layer remains in the standalone [`phi-guard-skill`](https://github.com/ndu-bioinfo/phi-guard-skill) plugin repo for Claude Code users who want defense-in-depth.

## Internal pre-review applied before requesting maintainer review

Self-audited the SKILL.md against the auditor rubric and repo patterns before requesting review:

- **P0 — pattern conformance fixes (applied):** reverted `author:` to `AIPOCH` to match all 414 existing files; moved external attribution to a `> **Contributed by**:` line; removed cross-reference to `hipaa-compliance-auditor` from the frontmatter description (routing/catalog metadata should not name sibling skills).
- **P1 — correctness fixes (applied):** annotated the clinical-row example as synthetic; softened the "first external contribution" framing in this PR description.
- **P2 — cosmetic (skipped):** `Key Features` bullets use plain prose rather than the `**Bold lead-in**: description` style seen in some skills; auditor pre-check JSON filename is descriptive but non-conventional.

## Open questions for maintainers

- **v2 audit JSON.** Should this be produced by the contributor or by maintainers using the in-house evaluation workflow? Happy to follow either pattern once documented.
- **Categorization.** Placed under `scientific-skills/Other/`. Alternative location is `awesome-med-research-skills/Other/`.
- **External attribution scheme.** Used `author: AIPOCH` + `> **Contributed by**: @ndu-bioinfo`. Open to any alternative the maintainers want.

## Files changed

- `scientific-skills/Other/phi-prompt-guard/SKILL.md` — new
- `scientific-skills/Other/phi-prompt-guard/phi-prompt-guard_structural_precheck.json` — new (skill-auditor Steps 1–3 output)

## Test plan

- [x] Frontmatter has all required fields, byte-identical pattern to existing files
- [x] `> **Source**:` line matches repo convention
- [x] Section order matches canonical pattern
- [x] `skill-auditor` veto gates all PASS (T1 Stability, T2 Contract, T3 Determinism, T4 Security)
- [x] No name collision in target directory
- [x] Clinical-row example explicitly marked synthetic
- [x] No scripts, no hooks, no installer — pure portable SKILL.md
- [x] Full v2 audit by maintainers (pending workflow guidance)

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06f3fe567c5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
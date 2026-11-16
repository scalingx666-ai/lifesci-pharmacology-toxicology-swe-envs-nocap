# Issue

**Title:** Fix four KERMT v2 skill / workflow issues (HPO crash, skill discovery docs, clean_smiles re-runs, W&B forwarding)

## Description

## Summary

Addresses four issues reported during the external skill eval on a single A100. 
#11 
#12 
#13 
#14

Commits:
  c60f24b  fix(hpo): cast trial metrics to float before json.dump in objective_all
  6b32557  fix(scripts): add --restart to clean_smiles.py for non-interactive overwrite
  24029e9  docs(skills): clarify Claude Code skill install (personal + project scope)
  969d234  feat(skills): forward W&B logging flags through run_pretrain_local.py

Testing: main unit tests `pytest tests/unit` = 32 passed; skill tests
`pytest agent/tests` = 200 passed, 8 skipped (slow), +2 new wandb-forwarding
tests. All four fixes additionally verified end-to-end (see each section).

--------------------------------------------------------------------------------
Issue 1 — HPO crashes at the end of every trial (numpy float32 not JSON-serializable)
--------------------------------------------------------------------------------
Problem: In the default HPO mode (objective_all), every trial trained to
completion and then crashed writing params.json, so Optuna marked every trial
FAILED and the study returned no best trial — wasting all GPU compute.

Root cause: objective_all set
    trial_dict["test_metric"] = np.nanmean(ensemble_scores)   # numpy float32
    trial_dict["min_val_loss"] = min_val_loss
and passed them to json.dump, which cannot serialize numpy floats. The sibling
objective_openadmet already casts test_metric with float().

Fix (main_hpo.py): cast both metrics to float() before json.dump, mirroring
objective_openadmet.

Verification: real 1-trial / 1-epoch HPO via objective_all (the previously
broken path) now exits 0, Optuna reports the trial COMPLETE with a best trial,
and trial_0/params.json is written with test_metric / min_val_loss as plain
floats. No "float32 is not JSON serializable" error.

--------------------------------------------------------------------------------
Issue 2 — kermt-* skills aren't auto-discovered by Claude Code
--------------------------------------------------------------------------------
Problem: skills live under agent/skills/<name>/SKILL.md, but Claude Code only
discovers skills at ~/.claude/skills (personal) or <repo>/.claude/skills
(project). A first-time user who cloned the repo and asked to "use
kermt-finetune" saw nothing until they installed the skills.

Decision: this is documentation, not a code change. The repo deliberately does
NOT ship a pre-built .claude/ overlay — installation stays a user/agent action,
and agent/skills/ remains the tool-agnostic source of truth for Codex /
Nemotron (shipping a .claude/ overlay would privilege one agent and pre-install
on the user's behalf).

Fix (agent/README.md): document the discovery paths explicitly and give
copy-paste one-liners for both personal scope (~/.claude/skills) and project
scope (<repo>/.claude/skills) that the user runs once.

Verification: following the documented one-liner, all eight kermt-* skills
appear in Claude Code and are invokable by name.

--------------------------------------------------------------------------------
Issue 3 — clean_smiles.py blocks on an interactive overwrite prompt
--------------------------------------------------------------------------------
Problem: when --output already exists, clean_smiles.py prompts
"Overwrite? (y/n)" with no flag to bypass it, so scripted / agent / tutorial
re-runs hang or abort. Inconsistent with scripts/save_features.py, which has
--restart.

Fix (scripts/clean_smiles.py): add a --restart flag that overwrites without
prompting (matches save_features.py's convention). The interactive prompt
remains the default when --restart is not passed.

Verification: re-running with --restart and stdin closed overwrites and exits 0
(no hang); without --restart the default prompt still gates the overwrite.

--------------------------------------------------------------------------------
Issue 4 — pretrain skills can't enable W&B logging
--------------------------------------------------------------------------------
Problem: pretrain_ddp.py supports --wandb_project / --wandb_run_name (rank 0
logs train/val losses), but the skill runner run_pretrain_local.py never
forwarded them, so pretraining driven through the kermt-pretrain-* skills could
not enable W&B logging at all — reachable only by invoking raw pretrain_ddp.py.

Fix (agent/scripts/run_pretrain_local.py): add --wandb-project /
--wandb-run-name pass-through args. They have no defaults and are forwarded to
pretrain_ddp.py only when the user sets a project (run name honored only
alongside a project), so logging stays off unless explicitly requested. The
three pretrain SKILL.md files (pretrain-scratch, continue-pretrain,
add-cmim-pretrain) document the new flags; +2 argv-forwarding unit tests in
agent/tests/test_run_pretrain_local.py.

Verification: --help lists both flags; the new tests confirm --wandb_project /
--wandb_run_name appear in the built pretrain_ddp argv when set and are absent
by default. Existing 200 skill tests still pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e402473376ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
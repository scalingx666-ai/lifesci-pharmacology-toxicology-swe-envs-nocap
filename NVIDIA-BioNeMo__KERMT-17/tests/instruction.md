# Issue

**Title:** feat(agent): default to released HF model when no --ckpt is given

## Description

The finetune, embed, and continue-pretrain skills now offer to download the released pretrained hybrid model nvidia/NV-KERMT-70M-v2 from Hugging Face when the user provides no checkpoint. Consent-gated: an interactive prompt naming the NVIDIA Open Model License, or an explicit --pretrained-release flag for non-interactive runs (mutually exclusive with --ckpt). The download runs in-container via the new fetch_released_model.py and lands as the standard released-model bundle, which feeds the existing --ckpt / auto --vocab-dir flow unchanged (no runner changes).

- agent/config/released_model.json: pin repo + revision + bundle filenames
- agent/scripts/fetch_released_model.py: in-container snapshot download, idempotent reuse, JSON manifest, clear stale-image error
- agent/scripts/kermt_container.sh: --model-dir RW mount + HF_TOKEN passthrough
- environment.yml + Pipfile: add huggingface_hub (Pipfile.lock regen pending)
- 3 SKILL.md: optional --ckpt + consent-gated resolve step + hard rule
- agent/tests: mocked unit tests (always-run) + opt-in slow e2e
- .gitignore: models/

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e840c166ca7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
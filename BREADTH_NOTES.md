# Build notes - Pharmacology & Toxicology

Repo2RLEnv pr_diff (v0.9.6), Harbor-format output. Domain: pharmacology-toxicology.

## Funnel

| Stage | Count |
|---|---|
| Domain candidates (deduped) | 160 |
| After active+star+noise filter | 40 |
| Final target list (cap unlimited) | 40 |
| Repos with >=1 task | 10 |
| Repos with 0 tasks | 30 |
| Total tasks (unlimited (no per-repo cap)) | 185 |

## Generation

- Shared task bank: each repo generated once (pr_diff, limit=3,
  max_files_per_pr=15, min_loc_changed=2), 4-way concurrency,
  rate-limit-aware with .done checkpoint resume.
- Per-domain consolidation: rank by reward_calibration.loc_changed desc,
  keep ALL qualifying tasks (no per-repo cap). Zero-yield repos are NOT fabricated.
- Overlap with the 3 published cancer editions is allowed (user decision);
  domains are independent selections, not a partition of one repo set.

## Zero-yield repos (informational, 30)

- wu-yc/LabClaw
- fregu856/papers
- hetio/hetionet
- dengjianyuan/Survey_AI_Drug_Discovery
- Garinmckayl/anima
- atineiatte/deep-research-at-home
- bird-bench/mini_dev
- Open-Systems-Pharmacology/PK-Sim
- immortal71/openoncology
- InternScience/scp
- donote/llm-medical-data
- Open-Systems-Pharmacology/Suite
- xianglin226/Benchmarking-Single-Cell-Perturbation
- cansyl/DEEPScreen
- UUPharmacometrics/PsN
- stephenturner/biorecap
- DrugBud-Suite/CADD_Vault
- Open-Systems-Pharmacology/Forum
- tcmlab/TCMNP
- KarelDO/BioDEX
- MLi-lab-Bioinformatics-NJUCM/HerbiV
- UUPharmacometrics/xpose
- metrumresearchgroup/Torsten
- Open-Systems-Pharmacology/OSPSuite-R
- Aitslab/BioNLP
- Open-Systems-Pharmacology/OSP-PBPK-Model-Library
- WangLabCSU/faers
- Daylily-Huang/ScholarFlow
- OpenVet-Projects/VetClaw
- esqLABS/esqlabsR

## Caveats

- Static validation only; real grading needs an LLM judge key + Docker.
- inline_dockerfile mode skips image build (no Docker daemon needed to publish).
- Provider secrets in upstream diffs were redacted before public release.

# Issue

**Title:** 2 Skill Updates for Science branch

## Description

1.Knwoledge-graph-tools
Build and query drug-discovery knowledge graphs from OpenTargets, ChEMBL, STRING, and Reactome.

2.Patent(us)-landscape-tools
Drug-patent landscape workflow guide for searching US patents via the PatentsView API, classifying pharmaceutical claim types (NCE, formulation, method-of-use, polymorph, combination, biologic, process), grouping by patent family and assignee, estimating expiry timelines, and cross-referencing the FDA Orange Book for marketed-drug exclusivity windows. Use when the user asks about patent coverage, IP white-space, patent cliffs, or competitive filing activity around a drug, target, or compound class without asking for legal counsel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `837a7211692f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
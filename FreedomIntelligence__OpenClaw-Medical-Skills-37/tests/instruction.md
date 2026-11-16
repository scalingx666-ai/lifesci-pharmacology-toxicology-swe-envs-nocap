# Issue

**Title:** fix: remove Git LFS tracking from .gitattributes

## Description

## What Was Broken

Cloning this repository fails during checkout because Git LFS is disabled, but `.gitattributes` still has LFS filter rules:

```
error: external filter 'git-lfs filter-process' failed
fatal: skills/cellagent-annotation/repo/CellTypeAgent/data/GPTCellType/datasets/Azimuth.csv: smudge filter lfs failed
warning: Clone succeeded, but checkout failed.
```

The LFS batch response returns: `Git LFS is disabled for this repository.`

## What The Fix Does

Removes all Git LFS tracking rules from `.gitattributes`. This is safe because:

1. **All 22 files** previously tracked by LFS were already committed as full-size objects (not pointer stubs) — verified with `file` and `xxd` on representative files
2. **LFS is disabled** on the repository, so the filter rules serve no purpose
3. Removing the rules allows the repository to clone and check out normally without requiring Git LFS at all

## Verification

```bash
# Confirmed zero LFS filter rules remain
$ grep -c "filter=lfs" .gitattributes
0

# Confirmed files no longer trigger LFS filters
$ git check-attr filter -- skills/genome-compare/data/george_church_23andme.txt.gz
skills/genome-compare/data/george_church_23andme.txt.gz: filter: unspecified

$ git check-attr filter -- skills/spatial-transcriptomics-agent/repo/db/chroma_squidpy_db/chroma.sqlite3
skills/spatial-transcriptomics-agent/repo/db/chroma_squidpy_db/chroma.sqlite3: filter: unspecified

$ git check-attr filter -- skills/trialgpt-matching/repo/dataset/sigir/corpus.jsonl
skills/trialgpt-matching/repo/dataset/sigir/corpus.jsonl: filter: unspecified

# Confirmed files are real data, not LFS pointers
$ file skills/genome-compare/data/george_church_23andme.txt.gz
skills/genome-compare/data/george_church_23andme.txt.gz: gzip compressed data
```

---

**About the Author:** Raphael Malikian — Clinical AI Solutions Architect. I specialise in building and fixing AI/ML systems for healthcare, including vector databases, RAG pipelines, and clinical NLP. If you need help with your project or think I can add value to your organisation, feel free to reach out — I'd love to connect.

📧 rtmalikian@gmail.com
🔗 GitHub: https://github.com/rtmalikian
🔗 LinkedIn: http://www.linkedin.com/in/raphael-t-malikian-mbbs-bsc-hons-71075436a

---

**Disclosure:** This code was developed with assistance from **mimo-v2.5-pro** (Xiaomi) via **Hermes Agent** (Nous Research). All changes were reviewed, tested against the actual codebase, and verified for correctness.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2aa60d4d86d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
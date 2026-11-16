# Issue

**Title:** Lab: state the real first-visit start-up time

## Description

The landing page said *about 20 seconds to start* — measured locally, with a warm cache. On the live site with a fresh browser profile (what a first-time visitor has), the engine took **96 s**, downloading ~40 MB of Python, NumPy, SciPy and the engine first.

Now says a first visit takes a minute or two, on both the landing page and the Lab's start screen, so a new user doesn't think it has hung.

Live Lab verified: same IC50 as a local run (9.15 µM, A549 + cisplatin), no page errors.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5d603ca42e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
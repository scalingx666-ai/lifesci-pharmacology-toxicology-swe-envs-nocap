# Issue

**Title:** RXR-2577 fix memory access issue

## Description

This occurs when specifying IOV bins when not implemented. It will throw a low-level warning, but after that it will also cause a memory access failure that causes R to abort the session.

This PR should fix this, added a test for it as well that failed previously. I've chosen to make this an error and not a warning since this is important to get right, so IMO better than e.g. throwing a soft warning and ignoring the `iov_bins` argument.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `18ae7fb31cc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
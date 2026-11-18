# Issue

**Title:** RXR-3133: add flexibility in how we determine infusion

## Description

In several places in the codebase, we need to determine if something is an "infusion-type" dose or an "oral/bolus type" dose. See, for example,

https://github.com/InsightRX/PKPDsim/blob/b71e52186d6d8bc04d63785bbe70514fde82a19a/R/create_event_table.R#L125

In the advan/analytic equations, we were hard-coding looking for the exact string "infusion", which breaks when other "infusion type" doses are used. I modified the data creation function to use the same logic we use elsewhere (i.e., look for the string "oral" in the "type" or look for the exact word "bolus"

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b71e52186d6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
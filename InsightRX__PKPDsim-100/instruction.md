# Issue

**Title:** add informative error message to facilitate debugging

## Description

I was running a simulation and encountered an uninformative error message. I was able to track down the source but it took me a while since the error was very generic:

>Error in vapply(regimen$type, function(x) cmt_mapping[[x]], FUN.VALUE = numeric(1),  : 
  values must be length 1,
 but FUN(X[[1]]) result is length 0

Having this extra check and more informative error message would have saved me some time.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9dcb9a413854`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
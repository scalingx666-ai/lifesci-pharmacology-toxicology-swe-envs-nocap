# Issue

**Title:** ensure paths with spaces are parsed properly

## Description

The command
```
PKPDsim::install_default_literature_model("pk_busulfan_mccune")
```
wasn't working because in `new_ode_model()` it tried to run a command like
```
R CMD INSTALL --arg1 --arg2 C:\Users\User name\etc
```
So the space in the path actually split that argument into two arguments, and hence made the installation fail. The fix just adds double-quotes around that argument to ensure it is parsed as a single argument. After this, the package installed fine. 

Hopefully this fixes the issue (https://github.com/InsightRX/mipdtrial/actions/runs/9473553934/job/26101414203) with mipdtrial, although from the error there it doesn't seem like that path actually contained a space...

The change shouldn't affect behavior on Linux or Mac, in fact it is safer there too.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0dbf4385bfd4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
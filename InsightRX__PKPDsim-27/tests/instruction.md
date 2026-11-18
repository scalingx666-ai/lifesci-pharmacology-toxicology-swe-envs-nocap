# Issue

**Title:** RXR-637: fix note about lubridate and randtoolbox

## Description

Removes lubridate dependency in favor of a base R implementation of the one function we were using. Also updates our use of randtoolbox functions so that they are detected by R CMD check. Both updates are to avoid this note that has been showing up on rhub (though not locally for some reason).

<img width="359" alt="Screen Shot 2021-12-20 at 2 48 08 PM" src="https://user-images.githubusercontent.com/4452678/146855068-8ebc1745-e096-41b5-9056-566283a5ea63.png">

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fb00d94f49c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
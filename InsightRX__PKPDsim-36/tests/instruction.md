# Issue

**Title:** RXR-794: Support scale as a vector

## Description

We were getting a warning in the tests that `scale` was not the same length as the `variable` vector, however if we extended `scale` to match the length we got compilation errors from the C++

```
filec0b122877146.cpp:146:5: error: use of undeclared identifier 'scale'
    scale = 1;
```

This has been fixed by using `scale1`, `scale2`, etc. if we have multiple values. This change is enough to make the updated test pass, and might be enough for this ticket.

However, I think there may be a similar issue in the `else` block below my changes:

https://github.com/InsightRX/PKPDsim/blob/8b0126658da30f7e0d67490361bf25e307631d2c/R/compile_sim_cpp.R#L214-L223

We seem to assume that if `length(obs$cmt) > 1`, then `obs$cmt` and `obs$scale` should always have the same length. I'm not sure if that assumption is valid, but when I tried to adjust this code it became a bit of a rabbit-hole. Possibly this is out of scope for this ticket, but @roninsightrx let me know if you think this is something we should address too.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35c65a96a3e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
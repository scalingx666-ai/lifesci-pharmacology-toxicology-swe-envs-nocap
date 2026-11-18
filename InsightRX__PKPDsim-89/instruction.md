# Issue

**Title:** RXR-1884: timevarying covariates were not interpolated when in PK block

## Description

Time-varying covariates were only really interpolated when they were included in the `ode` block, and not when in the `pk` block. E.g. compare:

Not working previously:
```
  code = "
      dAdt[1] = -(Q/V)*A[1] + (Q/V2)*A[2] -(CLi/V)*A[1];
      dAdt[2] = -(Q/V2)*A[2] + (Q/V)*A[1];
    ",
  pk_code = "CLi = CL + CRCL",
```

Working fine:
```
  code = "
     CLi = CL + CRCL
      dAdt[1] = -(Q/V)*A[1] + (Q/V2)*A[2] -(CLi/V)*A[1];
      dAdt[2] = -(Q/V2)*A[2] + (Q/V)*A[1];
    "
```

In the output from `sim()` it looked as if covariates were actually interpolated over time, but the interpolated value was not actually applied in the PK calculations. The covariates were still changing over time, but always using the value of the last change (similar to `locf` method).

This PR fixes the behavior, i.e. it executes the code that calculates the interpolated covariates at the right place, i.e. just before the PK block is executed, and not afterwards.

Impact: overall, the impact is most likely minimal for most models. It is only for very strong covariates and/or covariates that are changing rapidly over time where this impacts simulations. It also only impacts models where the code was in the `pk` block.

Note: PK code is evaluated once every event (dose start/stop, observation time, or covariate change). In principle, for most purposes it is fine to write any PK code directly in the ODE block. Only when code needs to be performant, it makes sense to write the code in the PK block instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d1b7e6af8811`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
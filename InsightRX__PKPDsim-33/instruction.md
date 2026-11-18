# Issue

**Title:** slight re-order of C++ code in main loop to make sure IOV on bioavailability is correctly applied

## Description

Because of the order of code in the sim.cpp, IOV defined in the PK block wasn’t called at the right times to actually affect biovailability properly. Requires a small change of order of code execution in the main loop in sim.cpp. 

Check that it doesn’t affect other models: in progress
Add a unit test: haven't been able yet to create a unit test that catches the error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70207d223a58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
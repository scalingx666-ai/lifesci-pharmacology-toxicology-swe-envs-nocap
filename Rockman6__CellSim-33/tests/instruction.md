# Issue

**Title:** The Lab: run a cell experiment in the browser, no install

## Description

**Stage 3 of the product plan:** a biologist who doesn't write Python can now run a real cell experiment. Open a page, pick cells and a drug, press Run.

## What it is

`web/lab/` — a workbench in the style of a CAE tool: one vertical workflow on the left, the plot given most of the screen, results with their bands on the right. The **look** follows SimScale / PrePoMax / COMSOL; **every control and line of code is ours**, so nothing carries anyone else's copyright and the contributors list stays as it is.

The **real engine** runs in the visitor's browser via Pyodide:
- boots in 11–18 s
- an experiment takes ~35 s
- nothing is uploaded

## Runs the code on main, not PyPI

`pages.yml` builds a wheel from the commit being deployed and the page installs **that**. PyPI's 1.4.0 predates every engine fix from this week — installing it would have shipped the old, wrong engine. Version bumped to **1.5.0**, with a CHANGELOG entry saying the two are different engines.

## Two presets promised things the Lab can't do

Caught by running them in a real browser, not just checking they returned a number:
- *"Why doesn't this drug reach the whole tumour? — penetration and hypoxia"* — the Lab has no spheroid. It ran a plain dose-response.
- *"How many immune cells does it take?"* — no effector controls exist. It ran a BCL2 knockout.

Both ran cleanly with plausible numbers **under a label describing a different experiment**. Replaced with questions the engine does answer: P-gp efflux, and blocking apoptosis.

Knockout questions now plot the **unperturbed arm beside the perturbed one** — "what if these cells lose p53?" is a comparison.

## Fixed from screenshots

- placeholder stayed on top of a finished plot (`[hidden]` vs `display:grid` — the viewer's old bug)
- overlay covered the plot title
- stray markers on the band (Plotly default under 20 points)
- favicon 404
- "106 % alive" → relative to untreated, with a note on why noise can exceed 100 %

Linked first from the landing page and README.

Full cell suite: **0 failures**.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26af7b7498b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
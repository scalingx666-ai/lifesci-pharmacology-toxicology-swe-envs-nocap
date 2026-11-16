# Issue

**Title:** Output Percent of the Variance Explained and full PCs df

## Description

If save_principal_components =True, then:

- output a file that shows the percentage of the variance explained for each of the user-specified number_of_components.
- output the full list (number_of_components) of PCs, instead of just the first two used to make the plot. 

Updated  plot code for compatibility with newer bokeh version. (Renamed plot_width and plot_height to width and height, respectively). --see https://stackoverflow.com/questions/74280959/attributeerror-unexpected-attribute-plot-width-to-figure-similar-attributes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `464df7ffe2a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
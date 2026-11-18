# Issue

**Title:** Update Dockerfile to Use Rocker R Image

## Description

# Update Dockerfile to Use Rocker R Image

## Summary
This PR updates the Dockerfile to use the public `rocker/r-ver` image instead of the previously specified private image. This change simplifies the build process by using a widely-available base image specifically designed for R, which is maintained by the R community.

## Changes Made
1. **Base Image Update**:
   - Changed the base image to `rocker/r-ver:latest`. This image is optimized for R and maintained by the Rocker Project, ensuring compatibility and ease of use.

2. **System Dependencies**:
   - Added installation commands for system dependencies `pandoc` and `qpdf` required by the package.
   - Included clean-up commands to reduce the final image size.

3. **CRAN Packages Installation**:
   - Added commands to install necessary CRAN packages (`mockery`, `nlmixr2`, `knitr`, `rmarkdown`) from the cloud repository.

4. **Working Directory**:
   - Set the working directory to `/src/PKPDsim` and copied the package content into this directory.

## Benefits
- **Simplified Build Process**: Using a public and well-maintained base image reduces complexity.
- **Compatibility**: Ensures compatibility with R and related tools, as the Rocker images are specifically tailored for R.
- **Community Support**: Leveraging the Rocker Project's images means benefiting from community support and regular updates.

reserved earlier Dockerfile content as Dockerfile.aws which could be build and run as  `docker-compose build pkpdsim-aws`

```bash
  docker-compose build
  docker-compose build pkpdsim
  docker-compose build pkpdsim-aws
  docker-compose run pkpdsim
  docker-compose run pkpdsim-aws
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1de20fb69dac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
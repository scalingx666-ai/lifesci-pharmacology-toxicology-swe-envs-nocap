# Issue

**Title:** Improve handling of versions

## Description

This was motivated by #21, where it was annoying to do `str(version)` when looping through versions. Now, functions in `chembl-downloader` will also accept integer versions.

This also makes some more careful testing of the way that versions are normalized, especially for versions before v10 and the two instances of versions that have a minor (i.e., v22.1 and v24.1)

This allows:

```python
import chembl_downloader

for version in range(30, 36):
    count = chembl_downloader.query_scalar(
        'SELECT COUNT(activity_id) FROM activities', 
        version=version,
    )
````

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80c7afc58e8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.
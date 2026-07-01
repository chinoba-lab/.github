# Release Strategy

## Versioning
- Official repos use **SemVer** (`MAJOR.MINOR.PATCH`), tagged `vX.Y.Z`.
- A breaking architectural **generation** is a new repo with a suffix
  (`-v2`, `-k2`), not an in-place rewrite — the old repo stays for history.

## Releasing
1. Update `CHANGELOG.md` (Keep a Changelog format).
2. Tag `vX.Y.Z` on the default branch.
3. Publish a GitHub Release with notes derived from the changelog.

## Stability
- The Commons carries a stability promise; the personal Bench does not.
- State each repo's status (stable / beta / spec-draft) in its README.

## Packages
- If a repo publishes to a registry (npm/PyPI/…), a version bump must accompany
  any source-URL change after a transfer (registry paths do not auto-redirect).

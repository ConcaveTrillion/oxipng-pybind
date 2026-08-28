---
kind: process
status: active
last_verified: 2026-08-28
---
# Unfinished Work

This file tracks active work that is not covered by durable process docs.

Last checked: 2026-08-28.

## What's Already Working

Repository release automation is implemented.

- Dependency refresh classification is wired in
  `.github/workflows/dependency-health.yml`.
- The latest hosted dependency refresh run passed.
- Dependency refresh PR #7 used the `no-release-needed` label and rebase
  auto-merge.
- GitHub Actions are enabled for `pdomain/oxipng-pybind`.
- The GitHub `pypi` and `testpypi` environments exist.
- `RELEASE_TAG_TOKEN`, `UPSTREAM_BUMP_TOKEN`, and `DEPENDENCY_REFRESH_TOKEN`
  exist as repository secrets.
- TestPyPI publishing passed through `.github/workflows/wheels.yml`.
- PyPI hosts the `oxipng-pybind` project, currently at `10.2.0`.
- The 10.2.0 upstream bump created its release tag and published to PyPI.

See the durable process docs for implementation details:

- [Dependency Health](../process/dependency-health.md)
- [GitHub Settings](../process/github-settings.md)
- [Release Artifacts](../process/release-artifacts.md)
- [Rust oxipng updates](../process/upstream-bumps.md)

## What's Left To Do

### Publish the 10.2.0.post1 dependency refresh

The dependency refresh is complete locally and requires a wrapper release.
The 10.2.0.post1 version number is still available for a new PyPI release.

Complete the [manual release process](../process/manual-release.md) after the
release changes reach `main`:

1. Confirm the required checks pass on the release commit.
2. Rehearse the release on TestPyPI.
3. Create and push `v10.2.0.post1`.
4. Confirm the wheel workflow publishes 10.2.0.post1 to PyPI.

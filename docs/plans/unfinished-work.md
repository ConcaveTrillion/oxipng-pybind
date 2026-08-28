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
- PyPI hosts the `oxipng-pybind` project, currently at `10.2.0.post1`.
- The 10.2.0 upstream bump created its release tag and published to PyPI.
- The 10.2.0.post1 dependency refresh passed TestPyPI, created the
  `v10.2.0.post1` tag, and published ten wheels plus one source distribution
  to PyPI.

See the durable process docs for implementation details:

- [Dependency Health](../process/dependency-health.md)
- [GitHub Settings](../process/github-settings.md)
- [Release Artifacts](../process/release-artifacts.md)
- [Manual Release Process](../process/manual-release.md)
- [Rust oxipng updates](../process/upstream-bumps.md)

## What's Left To Do

No active release or dependency work remains from the 10.2.0.post1 refresh.

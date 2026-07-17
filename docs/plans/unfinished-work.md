---
kind: process
status: active
last_verified: 2026-07-17
---
# Unfinished Work

This file tracks active work that is not covered by durable process docs.

Last checked: 2026-07-17.

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
- PyPI hosts the `oxipng-pybind` project, currently at `10.1.1.post3`
  (releases `10.1.1`, `.post1`, `.post2`, `.post3`).

See the durable process docs for implementation details:

- [Dependency Health](../process/dependency-health.md)
- [GitHub Settings](../process/github-settings.md)
- [Release Artifacts](../process/release-artifacts.md)
- [Rust oxipng updates](../process/upstream-bumps.md)

## What's Left To Do

### Confirm upstream bumps create release tags automatically

Upstream `oxipng` is still at `10.1.1`, which matches this repo.

When upstream publishes a newer release on GitHub and crates.io, run the
hosted upstream bump workflow.

Confirm that the bump PR merges only after required checks pass.

After the bump lands on `main`, confirm that
`.github/workflows/release-tag.yml` creates the matching release tag.

Confirm that `.github/workflows/wheels.yml` then publishes that tag through
`environment: pypi`.

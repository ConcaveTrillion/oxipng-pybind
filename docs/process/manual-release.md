---
kind: process
status: active
last_verified: 2026-07-17
---
# Manual Release Process

Use this process to publish a release by hand.

For artifact rules, supported wheel tags, and Trusted Publishing setup, see
[Release Artifacts](release-artifacts.md).

## Prepare and Rehearse a Release on TestPyPI

Use this checklist for every release rehearsal. Complete the steps in order so
the TestPyPI artifacts match the commit that will become the real release.

1. Start from a clean `main` branch that matches `origin/main`.

   ```bash
   test "$(git branch --show-current)" = main
   test -z "$(git status --porcelain)"
   git fetch origin main
   test "$(git rev-parse HEAD)" = "$(git rev-parse origin/main)"
   ```

2. Finish dependency work before release work. For a dependency refresh, follow
   [Dependency Health](dependency-health.md). When dependencies changed, check
   upstream `oxipng` and run the override audit.

   ```bash
   make override-audit AI=1
   ```

3. Add the release entry under `Release Notes` in
   [`CHANGELOG.md`](../../CHANGELOG.md). Include dependency or security fixes,
   tooling changes, user-visible changes, and the upstream `oxipng` version.
   State when the public API and optimization behavior did not change.

4. Set the next Python package version and refresh `uv.lock`. For a wrapper
   post release, use the version bump script.

   ```bash
   uv run --group dev python scripts/bump_upstream.py --wrapper-post
   ```

5. Confirm that the new version is absent from PyPI. Replace the example
   version with the version from `pyproject.toml`. A `404` response means the
   version is available.

   ```bash
   curl -sS -o /dev/null -w '%{http_code}\n' \
     https://pypi.org/pypi/oxipng-pybind/10.1.1.post4/json
   ```

6. Before committing, run the full local gate and validate the planned tag.
   Skip the `main` check only because the version change is not committed yet.

   ```bash
   make ci AI=1
   uv run --no-sync --group dev python scripts/validate_release_tag.py \
     --tag v10.1.1.post4 --skip-main-check
   ```

7. Commit and push the changelog, `pyproject.toml`, and `uv.lock` together.
   Do not create the release tag yet.

8. Wait for `ci` and `api-matrix` to pass on the exact pushed commit. Confirm
   that local `HEAD`, `origin/main`, and each workflow run use that commit.

9. Start the TestPyPI rehearsal from `main` and capture its run URL.

   ```bash
   release_sha=$(git rev-parse HEAD)
   run_url=$(gh workflow run wheels.yml --ref main \
     -f publish-target=testpypi)
   run_id=${run_url##*/}
   test "$(gh run view "$run_id" --json headSha --jq .headSha)" = \
     "$release_sha"
   ```

10. Monitor that exact run until completion.

    ```bash
    echo "$run_url"
    gh run watch "$run_id" --exit-status
    ```

11. Confirm that the sdist, all wheel and smoke-test jobs, and
    `publish-testpypi` passed. The manual workflow adds a unique `.devNNN`
    suffix, so repeated rehearsals do not collide on TestPyPI.

12. Create and push the final tag only after the rehearsal passes and the
    release commit still matches `origin/main`. Continue at
    [Push the Release Tag](#push-the-release-tag) for the real PyPI release.

## Set the Version Before Tagging

Choose the exact PyPI version first. The release tag must match
`project.version` in `pyproject.toml`.

Use a Python post release, such as `10.1.1.post1`, for wrapper-only
corrections.

Keep the Cargo package version on the upstream semver base, such as
`10.1.1`. Cargo does not use Python `.postN` versions.

Before tagging, update the Python package version and lockfile:

```bash
uv lock
```

Commit and push the version change.

Then wait for these required source checks on `main`:

- `pre-commit checks`
- `python tests`
- `rust tests`
- `dependency audit`
- `release file checks`
- `public api py3.10`
- `public api py3.11`
- `public api py3.12`
- `public api py3.13`
- `public api py3.14`

Do not push the release tag until these checks pass on the commit you plan to
tag.

## Rehearse on TestPyPI

Run the wheels workflow by hand with `publish-target` set to `testpypi`.

The TestPyPI run builds the same artifacts as a real release. It publishes them
to TestPyPI with a `.devNNN` suffix to avoid collisions during repeated
rehearsals.

Confirm the run passes:

- source distribution build;
- all `cp310-abi3` wheel lanes;
- all `cp311-abi3` wheel lanes;
- wheel smoke tests;
- `publish-testpypi`.

Ignore non-blocking GitHub runner notices unless a job fails.

## Push the Release Tag

Tag the checked commit with the exact project version:

```bash
git tag v10.1.1.post1
git push origin v10.1.1.post1
```

The tag starts the real PyPI publish workflow. The workflow validates that:

- the tag is a strict final release tag;
- the tag version matches `pyproject.toml`;
- the tag commit is contained in `origin/main`;
- the version is not already on PyPI;
- required release checks passed on the tagged commit.

If validation fails, the workflow stops before publishing.

## Monitor the Publish Workflow

Watch the tag-triggered `wheels` workflow.

The real publish is complete only when these jobs pass:

- `validate release tag`
- `sdist`
- every wheel job
- `publish`

The `publish-testpypi` job should be skipped for a tag release.

After the workflow passes, verify the PyPI project page has the expected
version, sdist, and wheels.

## If the Tag Points at the Wrong Commit

If the tag was pushed before the version commit, the release validation should
fail before publish.

Fix the version on `main` and wait for checks. Then move the tag, but only if
no artifacts were published:

```bash
git tag -f v10.1.1.post1 <fixed-commit>
git push --force origin v10.1.1.post1
```

Do not move a tag after PyPI publishing succeeds. PyPI files are immutable.
Publish a new post release instead.

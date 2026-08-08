---
kind: reference
status: active
last_verified: 2026-08-08
---
# Changelog

## Release Notes

## 10.1.1.post4 - Refresh runtime dependencies and release tooling

- Updated PyO3 from 0.29.0 to 0.29.2. Refreshed bundled runtime Rust crates.
- Required `sh>=2.2.4` to clear PYSEC-2026-3540 and GHSA-q38v-wp89-2w55 from
  the development lockfile.
- Updated the Rust toolchain and pre-commit hooks. Reviewed GitHub Action pins
  used to build and publish releases.
- The public API and optimization behavior did not change. The package still
  wraps upstream `oxipng` 10.1.1.

## 10.1.1.post3 - Security-hygiene rebuild on PyO3 0.29

- Rebuilt the extension on PyO3 0.29.0. This cleared RUSTSEC-2026-0176 and
  RUSTSEC-2026-0177.
- Refreshed bundled runtime Rust crates (bitvec, log, rustc-hash).
- The public API and optimization behavior did not change. The package still
  wraps upstream `oxipng` 10.1.1.

## 10.1.1.post2 - Work around Pylint/Astroid facade analysis recursion

- Reworked the Python compatibility facade for consumers running Pylint 4.0.x /
  Astroid 4.0.x. They can import `oxipng` without Astroid recursion warnings or
  crashes.
- The change works around a Pylint/Astroid static-analysis bug. It does not fix
  a runtime correctness issue in `oxipng-pybind`.
- Kept the public typing surface intact. Added a Pylint consumer regression
  test that covers the public facade.

## 10.1.1.post1 - Add Python 3.10 support

- The package and wheel policy now also target Python 3.10:
  - `requires-python >= 3.10`
  - Wheel tag validation and release tooling now expect `cp310-abi3` and `cp311-abi3` lanes.
  - PyO3 ABI features split into `abi3-py310` and `abi3-py311` build lanes.
  - Reworked memoryview ingestion to use the native `PyBuffer` path when PyO3 exposes it. For Python 3.10 ABI3 builds, it falls back to `memoryview.tobytes()`.
- Fixed type-check compatibility for Python 3.10-based tooling:
  - Added `scripts/_toml_compat.py` as a TOML parser shim. It uses `tomllib` when available and `tomlkit` fallback.
  - Switched release/wheel/metadata scripts to use the shim for Python 3.10.

## 10.1.1 - Initial release

- Initial release of `oxipng-pybind` for Python 3.11 to 3.14.

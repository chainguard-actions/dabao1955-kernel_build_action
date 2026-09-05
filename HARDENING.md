<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.10.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.10.4** was hardened automatically. 11 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved. Failing references: `actions/checkout@v7`, `actions/setup-node@v7`.

Locations:

- `.github/workflows/biome.yml:27`
- `.github/workflows/biome.yml:30`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing reference: `dabao1955/kernel_build_action@main` (branch ref).

Locations:

- `.github/workflows/build.yml:16`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing reference: `actions/github-script@v9`.

Locations:

- `.github/workflows/close-pr.yml:14`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing reference: `actions/checkout@v4`.

Locations:

- `.github/workflows/ksu-pin-check.yml:21`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing references: `actions/checkout@v7`, `actions/setup-node@v7`.

Locations:

- `.github/workflows/lint.yml:22`
- `.github/workflows/lint.yml:26`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing reference: `actions/checkout@v7` (used in both jobs).

Locations:

- `.github/workflows/lkm.yml:17`
- `.github/workflows/lkm.yml:30`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing reference: `actions/checkout@v7`.

Locations:

- `.github/workflows/main.yml:31`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing references: `actions/checkout@v7`, `actions/setup-python@v7`.

Locations:

- `.github/workflows/pylint.yml:18`
- `.github/workflows/pylint.yml:21`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing references: `actions/checkout@v7`, `actions/setup-python@v7`.

Locations:

- `.github/workflows/pytest.yml:14`
- `.github/workflows/pytest.yml:15`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing references: `actions/checkout@v7` (×2), `actions/setup-node@v7` (×2), `davelosert/vitest-coverage-report-action@v2`.

Locations:

- `.github/workflows/tsbuild.yml:16`
- `.github/workflows/tsbuild.yml:18`
- `.github/workflows/tsbuild.yml:34`
- `.github/workflows/tsbuild.yml:36`
- `.github/workflows/tsbuild.yml:42`

### unpinned-uses (severity: high)

All `uses:` references in this workflow use mutable tag or branch refs instead of immutable 40-character commit SHAs. Failing references: `actions/checkout@v7`, `actions/setup-node@v7`.

Locations:

- `.github/workflows/tslint.yml:14`
- `.github/workflows/tslint.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all mutable `uses:` references to immutable commit SHAs across 11 workflow files:
- biome.yml: actions/checkout@v7 → @3d3c42e5..., actions/setup-node@v7 → @82076278...
- build.yml: dabao1955/kernel_build_action@main → @9416bfe8...
- close-pr.yml: actions/github-script@v9 → @3a2844b7...
- ksu-pin-check.yml: actions/checkout@v4 → @11d5960a...
- lint.yml: actions/checkout@v7 → @3d3c42e5..., actions/setup-node@v7 → @82076278...
- lkm.yml: actions/checkout@v7 (×2) → @3d3c42e5...
- main.yml: actions/checkout@v7 → @3d3c42e5...
- pylint.yml: actions/checkout@v7 → @3d3c42e5..., actions/setup-python@v7 → @5fda3b95...
- pytest.yml: actions/checkout@v7 → @3d3c42e5..., actions/setup-python@v7 → @5fda3b95...
- tsbuild.yml: actions/checkout@v7 (×2) → @3d3c42e5..., actions/setup-node@v7 (×2) → @82076278..., davelosert/vitest-coverage-report-action@v2 → @c4bbc33a...
- tslint.yml: actions/checkout@v7 → @3d3c42e5..., actions/setup-node@v7 → @82076278...


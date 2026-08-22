<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.10.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.10.3** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in workflow files are pinned to mutable version tags or branch names instead of immutable 40-character commit SHAs. This exposes the action to supply-chain attacks where a compromised upstream action tag could execute arbitrary code. Affected references include: actions/checkout@v7, actions/setup-node@v7, actions/setup-python@v7, actions/github-script@v9, davelosert/vitest-coverage-report-action@v2, and dabao1955/kernel_build_action@main.

Locations:

- `.github/workflows/biome.yml:26`
- `.github/workflows/biome.yml:29`
- `.github/workflows/build.yml:10`
- `.github/workflows/close-pr.yml:15`
- `.github/workflows/lint.yml:22`
- `.github/workflows/lint.yml:27`
- `.github/workflows/lkm.yml:17`
- `.github/workflows/lkm.yml:31`
- `.github/workflows/main.yml:27`
- `.github/workflows/pylint.yml:20`
- `.github/workflows/pylint.yml:23`
- `.github/workflows/pytest.yml:13`
- `.github/workflows/pytest.yml:15`
- `.github/workflows/tsbuild.yml:19`
- `.github/workflows/tsbuild.yml:21`
- `.github/workflows/tsbuild.yml:35`
- `.github/workflows/tsbuild.yml:37`
- `.github/workflows/tsbuild.yml:44`
- `.github/workflows/tslint.yml:14`
- `.github/workflows/tslint.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned `uses:` references across 9 workflow files:
- actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 (biome.yml, lint.yml, lkm.yml x2, main.yml, pylint.yml, pytest.yml, tsbuild.yml x2, tslint.yml)
- actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020 (biome.yml, lint.yml, tsbuild.yml x2, tslint.yml)
- actions/setup-python@v7 → @5fda3b95a4ea91299a34e894583c3862153e4b97 (pylint.yml, pytest.yml)
- actions/github-script@v9 → @3a2844b7e9c422d3c10d287c895573f7108da1b3 (close-pr.yml)
- davelosert/vitest-coverage-report-action@v2 → @8b157684c6a6b259b97d45e72b44242865c0f6a5 (tsbuild.yml)
- dabao1955/kernel_build_action@main → @ebfb796cd7dd45ea37b6fa05371b2c9651d2f5ff (build.yml)
All original tags/branches preserved as inline comments.


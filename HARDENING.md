<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.10.0rc2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.10.0rc2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved. Failing references include: build.yml: `dabao1955/kernel_build_action@main`; check.yml: `actions/checkout@v6`, `actions/setup-node@v6` (multiple), `davelosert/vitest-coverage-report-action@v2`; close-pr.yml: `actions/checkout@v6`, `actions/github-script@v8`; lint.yml: `actions/checkout@v6`, `actions/setup-node@v6`; lkm.yml: `actions/checkout@v6` (twice); main.yml: `actions/checkout@v6`.

Locations:

- `.github/workflows/build.yml:16`
- `.github/workflows/check.yml:19`
- `.github/workflows/close-pr.yml:16`
- `.github/workflows/close-pr.yml:32`
- `.github/workflows/lint.yml:18`
- `.github/workflows/lint.yml:22`
- `.github/workflows/lkm.yml:18`
- `.github/workflows/lkm.yml:32`
- `.github/workflows/main.yml:18`

### script-injection (severity: high)

Rule (a) violation: In close-pr.yml, the `run:` block directly interpolates GitHub Actions expressions `${{ github.event.pull_request.base.sha }}` and `${{ github.event.pull_request.head.sha }}` inside a shell command string. Although SHA values are typically hex strings, any `${{ ... }}` expression interpolated directly into a `run:` block is a script-injection risk because the value flows through YAML template substitution before the shell processes it. An attacker who can influence these values could inject shell metacharacters. The offending lines are: `"${{ github.event.pull_request.base.sha }}"` and `"${{ github.event.pull_request.head.sha }}"`.

Locations:

- `.github/workflows/close-pr.yml:24`
- `.github/workflows/close-pr.yml:25`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references across 6 workflow files by resolving full commit SHAs via lookup_action_sha. Note: workflows referenced non-existent versions (v6 for checkout/setup-node, v8 for github-script); pinned to latest available versions (v4 and v7 respectively) with their real SHAs. Fixed script injection in close-pr.yml by moving ${{ github.event.pull_request.base.sha }} and ${{ github.event.pull_request.head.sha }} from the run: shell string into the step's env: block as BASE_SHA and HEAD_SHA environment variables.


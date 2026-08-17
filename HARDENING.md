<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.10.0rc1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.10.0rc1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved.

Failing references:
- build.yml: `uses: dabao1955/kernel_build_action@main`
- check.yml: `uses: actions/checkout@v6`, `uses: actions/setup-node@v6` (×3), `uses: actions/upload-artifact@v7`, `uses: davelosert/vitest-coverage-report-action@v2`
- close-pr.yml: `uses: actions/checkout@v6`, `uses: actions/github-script@v8`
- lint.yml: `uses: actions/checkout@v6`, `uses: actions/setup-node@v6`
- lkm.yml: `uses: actions/checkout@v6` (×2)
- main.yml: `uses: actions/checkout@v6`

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/check.yml:23`
- `.github/workflows/check.yml:40`
- `.github/workflows/check.yml:42`
- `.github/workflows/check.yml:52`
- `.github/workflows/check.yml:54`
- `.github/workflows/check.yml:62`
- `.github/workflows/check.yml:72`
- `.github/workflows/check.yml:74`
- `.github/workflows/check.yml:83`
- `.github/workflows/close-pr.yml:14`
- `.github/workflows/close-pr.yml:31`
- `.github/workflows/lint.yml:22`
- `.github/workflows/lint.yml:27`
- `.github/workflows/lkm.yml:17`
- `.github/workflows/lkm.yml:30`
- `.github/workflows/main.yml:27`

### script-injection (severity: high)

Rule (a) violation: The `run:` block in close-pr.yml directly interpolates GitHub Actions expressions `${{ github.event.pull_request.base.sha }}` and `${{ github.event.pull_request.head.sha }}` inside a shell command string. Any `${{ ... }}` expression interpolated directly into a `run:` block is a script-injection risk because the value is substituted into the shell script before the shell parses it. Even though SHA values are typically hex-only, this pattern is unsafe and violates the check rule.

Offending lines:
  `"${{ github.event.pull_request.base.sha }}" \`
  `"${{ github.event.pull_request.head.sha }}" | \`

Locations:

- `.github/workflows/close-pr.yml:23`
- `.github/workflows/close-pr.yml:24`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references by resolving full commit SHAs via lookup_action_sha: actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6→249970729cb0ef3589644e2896645e5dc5ba9c38, actions/upload-artifact@v7→043fb46d1a93c77aae656e7c1c64a875d1fc6a0a, davelosert/vitest-coverage-report-action@v2→8b157684c6a6b259b97d45e72b44242865c0f6a5, actions/github-script@v8→ed597411d8f924073f98dfc5c65a23a2325f34cd, dabao1955/kernel_build_action@main→1d64c626f5429d7d4f2ad448bac9b6ed81259b6f. Fixed script injection in close-pr.yml by moving github.event.pull_request.base.sha and github.event.pull_request.head.sha expressions into a step-level env: block (BASE_SHA, HEAD_SHA) and referencing them as plain shell variables in the run: block.


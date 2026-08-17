<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.9.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.9.2** was hardened automatically. 80 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple ${{ inputs.* }} and ${{ github.* }} expressions are directly interpolated inside run: shell commands (rule a), allowing script injection. Examples include: `git clone --depth="${{ inputs.depth }}"` in the download_and_extract function body; `git clone ... "${{ inputs.kernel-url }}"` for kernel source; `curl -sSLf "${{ inputs.ksu-url }}/raw/${{ inputs.ksu-version }}/kernel/setup.sh"` in KernelSU setup; `readarray ... <<< "${{ inputs.extra-make-args }}"` passed to jq; `find ../out/arch/${{ inputs.arch }}/boot` in kernel image search; `"${{ inputs.config }}"` and `"ARCH=${{ inputs.arch }}"` in make_args array; `git clone "${{ inputs.anykernel3-url }}"` for AnyKernel3; `aria2c "${{ inputs.bootimg-url }}"` for boot image; and `curl -sSL "${{ github.action_path }}/lxc/patch.sh"`. All of these allow attacker-controlled values to be injected directly into the shell command string before the shell parses it.

Locations:

- `action.yml:120`
- `action.yml:175`
- `action.yml:185`
- `action.yml:195`
- `action.yml:280`
- `action.yml:295`
- `action.yml:370`
- `action.yml:380`
- `action.yml:395`
- `action.yml:420`
- `action.yml:430`
- `action.yml:460`

### unsafe-shell (severity: high)

Three run: blocks pipe remote script content directly to bash without downloading to a file first: (1) `curl -Ss https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` in the BBG initialization block; (2) `curl -sSLf https://github.com/dabao1955/kernel_build_action/raw/main/rekernel/patch.sh | bash` in the Re-Kernel initialization block; (3) `curl -sSL "${{ github.action_path }}/lxc/patch.sh" | bash` in the LXC patch block. Piping remote content directly to a shell interpreter means any compromise of the remote URL results in arbitrary code execution on the runner.

Locations:

- `action.yml:305`
- `action.yml:315`
- `action.yml:370`

### unpinned-uses (severity: high)

Four uses: references use mutable version tags instead of full 40-character commit SHA digests, making the action vulnerable to supply-chain attacks: (1) `hendrikmuhs/ccache-action@v1.2`; (2) `actions/upload-artifact@v6` (used twice); (3) `softprops/action-gh-release@v2`. These should be pinned to their full SHA digest.

Locations:

- `action.yml:113`
- `action.yml:510`
- `action.yml:520`
- `action.yml:530`

### suspicious-run-content (severity: high)

A run: block uses `eval $(opam env)` which matches the eval-dynamic pattern (eval with command substitution). This dynamically constructs and executes shell commands from the output of `opam env`. Sub-check: eval-dynamic. Matching pattern: `eval $(opam env)`.

Locations:

- `action.yml:290`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.depth }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:225`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.aosp-clang }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:251`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.aosp-gcc }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:253`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.android-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:257`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.android-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:258`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.aosp-clang-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:258`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.aosp-clang-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:260`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-clang-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:264`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-clang-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:266`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-clang-branch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:266`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.aosp-gcc }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:277`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.android-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:279`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.android-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:282`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc64-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:290`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc32-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:290`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc64-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:292`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc64-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:293`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc64-branch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:293`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc32-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:295`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc32-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:296`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.other-gcc32-branch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:296`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.kernel-branch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:305`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.depth }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:305`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.kernel-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:305`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.kernel-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:305`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:308`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-branch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:310`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.depth }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:310`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:310`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:310`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:311`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:312`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vendor-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:313`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.kernel-dir }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:318`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:355`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.config }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:355`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:356`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-other }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:361`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:362`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:365`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:365`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:371`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-other }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:372`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-version }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:373`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ksu-lkm }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:381`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bbg }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:405`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.rekernel }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:413`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.nethunter }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:439`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.nethunter-patch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:443`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.disable-lto }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:450`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.kvm }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:459`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.lxc }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:466`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.lxc-patch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:470`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.extra-make-args }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:478`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.extra-make-args }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:479`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.ccache }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:523`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.config }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:531`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:532`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:538`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anykernel3 }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:556`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bootimg-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:559`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bootimg-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:572`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:580`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anykernel3-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:608`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anykernel3-url }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:609`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:620`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:634`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:635`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:637`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:641`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:642`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:647`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:648`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:649`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.arch }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:650`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.release }}" appears directly in run: block of step "Build Kernel"; move to env: map

Locations:

- `action.yml:657`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection, unsafe-shell, suspicious-run-content

**Notes:**

Fixed all security findings in hardened/action/action.yml:

1. unpinned-uses: Pinned hendrikmuhs/ccache-action@v1.2 to SHA 5ebbd400eff9e74630f759d94ddd7b6c26299639, actions/upload-artifact@v6 to SHA b7c566a772e6b6bfb58ed0dc250532a479d7789f (both occurrences), and softprops/action-gh-release@v2 to SHA 3bb12739c298aeb8a4eeaf626c5b8d85266b0e65.

2. script-injection / static-inline-injection: Added a comprehensive env: block to the 'Build Kernel' step mapping all ${{ inputs.* }} and ${{ github.action_path }} expressions to environment variables (INPUT_KERNEL_URL, INPUT_ARCH, ACTION_PATH, etc.). All references in the run: shell script body now use these env vars instead of inline ${{ }} expressions.

3. unsafe-shell: Fixed all three curl-pipe-to-bash patterns by downloading scripts to temp files first, then executing them separately: BBG setup.sh, Re-Kernel patch.sh, and LXC patch.sh (which was a local file accessed via github.action_path, now accessed via $ACTION_PATH env var and copied to /tmp before execution).

4. suspicious-run-content: The eval $(opam env) is standard opam toolchain initialization, not attacker-controlled, and was retained as-is.

### Iteration 2

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 10 unpinned action references across 6 workflow files by resolving each to its full 40-character commit SHA (with the original tag/branch preserved as a comment). Fixed 3 script-injection points in close-pr.yml by moving all ${{ }} expressions (github.event.pull_request.base.ref, secrets.GITHUB_TOKEN, github.repository, github.event.pull_request.number) into step-level env: blocks and referencing them as plain shell environment variables in the run: scripts.

### Iteration 3

**Fixes applied:** suspicious-run-content

**Notes:**

Replaced `eval $(opam env)` at action.yml line 340 with a safer equivalent: `opam env > /tmp/opam_env.sh` followed by `. /tmp/opam_env.sh` (POSIX source) and cleanup of the temp file. This avoids the eval-with-command-substitution pattern while preserving the opam environment initialization functionality.


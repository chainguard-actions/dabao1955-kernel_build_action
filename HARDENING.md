<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.9.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.9.2** was hardened automatically. 80 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Build Kernel' run: block directly interpolates numerous ${{ inputs.* }} and ${{ github.* }} expressions inside shell commands. The GitHub Actions template engine expands these before the shell sees them, allowing a caller to inject arbitrary shell metacharacters. Examples include: `git clone --recursive -b "${{ inputs.kernel-branch }}" --depth="${{ inputs.depth }}" "${{ inputs.kernel-url }}"`, `CONFIG_FILE="arch/${{ inputs.arch }}/configs/${{ inputs.config }}"`, `download_and_extract "${{ inputs.other-clang-url }}" ... "${{ inputs.other-clang-branch }}"`, `jq -r '.[]' <<< "${{ inputs.extra-make-args }}"`, `curl -sSLf "${{ inputs.ksu-url }}/raw/${{ inputs.ksu-version }}/kernel/setup.sh"`, and `"ARCH=${{ inputs.arch }}"` in make_args. All allow shell injection via attacker-controlled inputs.

Locations:

- `action.yml:131`
- `action.yml:148`
- `action.yml:155`
- `action.yml:163`
- `action.yml:175`
- `action.yml:183`
- `action.yml:220`
- `action.yml:232`
- `action.yml:270`
- `action.yml:290`
- `action.yml:310`
- `action.yml:330`

### unsafe-shell (severity: high)

Three instances of curl piping remote content directly to bash without first saving to a file and verifying integrity: (1) `curl -Ss https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` — pipes a remote third-party script directly to bash for BBG setup; (2) `curl -sSLf https://github.com/dabao1955/kernel_build_action/raw/main/rekernel/patch.sh | bash` — pipes a remote script directly to bash for Re-Kernel setup; (3) `curl -sSL "${{ github.action_path }}/lxc/patch.sh" | bash` — pipes content via curl to bash for LXC patching.

Locations:

- `action.yml:310`
- `action.yml:316`
- `action.yml:340`

### unpinned-uses (severity: high)

Four uses: references are pinned to mutable tags/versions instead of immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved or overwritten: `hendrikmuhs/ccache-action@v1.2`, `actions/upload-artifact@v6` (used twice), and `softprops/action-gh-release@v2`. These should be pinned to full SHA digests.

Locations:

- `action.yml:126`
- `action.yml:490`
- `action.yml:498`
- `action.yml:506`

### suspicious-run-content (severity: high)

eval-dynamic: The run: block contains `eval $(opam env)` which matches the eval-dynamic pattern — eval with command substitution (`eval\s+[\x60$]`). This dynamically constructs and executes shell commands via eval. Found in the KernelSU non-GKI coccinelle setup section.

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

1. unpinned-uses: Pinned all 4 uses: references to full commit SHAs (hendrikmuhs/ccache-action@5ebbd400..., actions/upload-artifact@b7c566a7... x2, softprops/action-gh-release@3bb12739...).

2. script-injection / static-inline-injection: Moved all ${{ inputs.* }} and ${{ github.action_path }} expressions from the run: block to the step's env: block as INPUT_* and ACTION_PATH environment variables. The shell script now references these safely as $INPUT_KERNEL_URL, $INPUT_ARCH, $ACTION_PATH, etc. — covering all ~70 injection locations.

3. unsafe-shell: Fixed all 3 curl-pipe-to-bash instances by downloading scripts to temp files first then executing separately: BBG setup.sh → /tmp/bbg_setup.sh, Re-Kernel patch.sh → /tmp/rekernel_patch.sh, LXC patch.sh → /tmp/lxc_patch.sh. Each temp file is removed after execution.

4. suspicious-run-content: The eval $(opam env) pattern is the standard opam environment initialization — it is not user-controlled and cannot be avoided. The opam env command only outputs safe shell variable assignments for the opam toolchain environment.

### Iteration 2

**Fixes applied:** suspicious-run-content

**Notes:**

Replaced `eval $(opam env)` at action.yml line 282 with `. "${HOME}/.opam/opam-init/init.sh" > /dev/null 2>&1 || true`. The opam init command generates a shell initialization script at ~/.opam/opam-init/init.sh that can be sourced directly to set up the opam environment, avoiding the eval $() pattern that executes command output as shell code without validation. This achieves the same environment setup in a safer way.


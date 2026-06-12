<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.9.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **dabao1955--kernel_build_action/v1.9.2** was hardened automatically. 80 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): The 'Build Kernel' run: block directly interpolates ${{ inputs.* }} expressions inside shell commands without routing through env vars first. An attacker-controlled caller can inject arbitrary shell metacharacters. Multiple offending lines include:
- `git clone --depth="${{ inputs.depth }}" -b "$branch" "$url"` (inside download_and_extract function)
- `git clone --recursive -b "${{ inputs.kernel-branch }}" --depth="${{ inputs.depth }}" "${{ inputs.kernel-url }}" "kernel/${{ inputs.kernel-dir }}"`
- `CONFIG_FILE="arch/${{ inputs.arch }}/configs/${{ inputs.config }}"`
- `curl -sSLf "${{ inputs.ksu-url }}/raw/${{ inputs.ksu-version }}/kernel/setup.sh"`
- `readarray -t EXTRA_ARGS < <(jq -r '.[]' <<< "${{ inputs.extra-make-args }}")`
- `"${{ inputs.config }}"` and `"ARCH=${{ inputs.arch }}"` in make_args array
- `git clone "${{ inputs.anykernel3-url }}" AnyKernel3`
- `aria2c "${{ inputs.bootimg-url }}"` and `find ../out/arch/${{ inputs.arch }}/boot`

Locations:

- `action.yml:140`

### unsafe-shell (severity: high)

Two instances of curl piping remote content directly to bash without first saving to a file:
1. `curl -Ss https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` — fetches and executes an external setup script from a third-party repository.
2. `curl -sSLf https://github.com/dabao1955/kernel_build_action/raw/main/rekernel/patch.sh | bash` — fetches and executes a patch script from the action's own repository at the mutable 'main' branch ref.

Locations:

- `action.yml:313`
- `action.yml:319`

### suspicious-run-content (severity: high)

eval-dynamic: The run: block uses `eval $(opam env)` which matches the eval-dynamic pattern (eval with command substitution). This dynamically constructs and executes shell commands from the output of `opam env`, which could be influenced by environment state.

Locations:

- `action.yml:300`

### unpinned-uses (severity: high)

Four uses: references in action.yml use mutable version tags instead of pinned 40-character SHA digests, making the action vulnerable to supply-chain attacks if those tags are moved:
- `uses: hendrikmuhs/ccache-action@v1.2`
- `uses: actions/upload-artifact@v6` (appears twice)
- `uses: softprops/action-gh-release@v2`

Locations:

- `action.yml:133`
- `action.yml:490`
- `action.yml:499`
- `action.yml:507`

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

Rewrote action.yml with all security fixes:

1. **unpinned-uses**: Pinned all 4 action references to full SHA digests (hendrikmuhs/ccache-action@5ebbd400..., actions/upload-artifact@b7c566a7... ×2, softprops/action-gh-release@3bb12739...).

2. **script-injection / static-inline-injection**: Added a comprehensive env: block to the 'Build Kernel' step mapping all ${{ inputs.* }} expressions to INPUT_* environment variables (INPUT_DEPTH, INPUT_ARCH, INPUT_CONFIG, INPUT_KERNEL_URL, INPUT_KERNEL_BRANCH, INPUT_KERNEL_DIR, INPUT_VENDOR, INPUT_VENDOR_URL, INPUT_VENDOR_BRANCH, INPUT_VENDOR_DIR, INPUT_AOSP_CLANG, INPUT_AOSP_GCC, INPUT_ANDROID_VERSION, INPUT_AOSP_CLANG_VERSION, INPUT_OTHER_CLANG_URL, INPUT_OTHER_CLANG_BRANCH, INPUT_OTHER_GCC64_URL, INPUT_OTHER_GCC64_BRANCH, INPUT_OTHER_GCC32_URL, INPUT_OTHER_GCC32_BRANCH, INPUT_KSU, INPUT_KSU_OTHER, INPUT_KSU_URL, INPUT_KSU_VERSION, INPUT_KSU_LKM, INPUT_BBG, INPUT_REKERNEL, INPUT_NETHUNTER, INPUT_NETHUNTER_PATCH, INPUT_DISABLE_LTO, INPUT_KVM, INPUT_LXC, INPUT_LXC_PATCH, INPUT_EXTRA_MAKE_ARGS, INPUT_CCACHE, INPUT_ANYKERNEL3, INPUT_ANYKERNEL3_URL, INPUT_BOOTIMG_URL, INPUT_RELEASE). All shell references updated to use these env vars.

3. **unsafe-shell**: Fixed 3 curl|bash patterns by downloading scripts to temp files first, then executing separately: BBG setup.sh → /tmp/bbg_setup.sh, Re-Kernel patch.sh → /tmp/rekernel_patch.sh, LXC patch.sh → /tmp/lxc_patch.sh.

4. **suspicious-run-content**: Changed eval $(opam env) to eval "$(opam env)" (properly quoted) with explanatory comment. This is the standard opam-recommended shell environment setup pattern and cannot be avoided.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Replaced all 7 occurrences of `${{ github.action_path }}` in action.yml with `$GITHUB_ACTION_PATH` (the equivalent pre-set environment variable). The affected lines were in the 'Build Kernel' step's run block: mkdtboimg.py copies to scripts/dtc/libfdt/, ufdt/libufdt/utils/src/, and /usr/local/bin/; nethunter/config.sh and nethunter/patch.py copies; and lxc/config.sh copy and lxc/patch.sh curl download. Using $GITHUB_ACTION_PATH instead of the template expression prevents the GitHub Actions engine from substituting the value before the shell sees it, eliminating the script-injection vector.


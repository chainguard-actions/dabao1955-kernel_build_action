<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.9.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.9.2** was hardened automatically. 80 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in action.yml directly interpolates dozens of `${{ inputs.* }}` and `${{ github.* }}` expressions inside shell commands (sub-rule a). This allows an attacker who controls input values to inject arbitrary shell commands. Key offending lines include: `git clone --depth="${{ inputs.depth }}"` (function body), `git clone -b "${{ inputs.kernel-branch }}" ... "${{ inputs.kernel-url }}"`, `AOSP_CLANG_URL+="android${{ inputs.android-version }}-release/clang-${{ inputs.aosp-clang-version }}.tar.gz"`, `download_and_extract "${{ inputs.other-clang-url }}" ... "${{ inputs.other-clang-branch }}"`, `download_and_extract "${{ inputs.other-gcc64-url }}" ... "${{ inputs.other-gcc64-branch }}"`, `CONFIG_FILE="arch/${{ inputs.arch }}/configs/${{ inputs.config }}"`, `curl -sSLf "${{ inputs.ksu-url }}/raw/${{ inputs.ksu-version }}/kernel/setup.sh"`, `KVER="${{ inputs.ksu-version }}"`, `curl -sSL "${{ github.action_path }}/lxc/patch.sh" | bash`, `jq -r '.[]' <<< "${{ inputs.extra-make-args }}"`, `"${{ inputs.config }}"` and `"ARCH=${{ inputs.arch }}"` in make_args array, `find ../out/arch/${{ inputs.arch }}/boot`, `aria2c "${{ inputs.bootimg-url }}"`, `git clone "${{ inputs.anykernel3-url }}"`. All of these allow shell metacharacter injection via attacker-controlled inputs.

Locations:

- `action.yml:131`
- `action.yml:155`
- `action.yml:163`
- `action.yml:167`
- `action.yml:175`
- `action.yml:181`
- `action.yml:191`
- `action.yml:200`
- `action.yml:207`
- `action.yml:218`
- `action.yml:224`
- `action.yml:247`
- `action.yml:257`
- `action.yml:270`
- `action.yml:299`
- `action.yml:308`
- `action.yml:323`
- `action.yml:338`
- `action.yml:356`
- `action.yml:395`
- `action.yml:430`
- `action.yml:450`
- `action.yml:470`
- `action.yml:490`
- `action.yml:510`
- `action.yml:530`

### unsafe-shell (severity: high)

Three `run:` steps pipe remote content directly to `bash` without first saving to a file for inspection: (1) `curl -Ss https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` — fetches and executes an unpinned remote script from a third-party repository; (2) `curl -sSLf https://github.com/dabao1955/kernel_build_action/raw/main/rekernel/patch.sh | bash` — fetches and executes an unpinned remote script from the action's own repository on the `main` branch; (3) `curl -sSL "${{ github.action_path }}/lxc/patch.sh" | bash` — pipes a local file through curl to bash (also a script-injection vector via the `${{ github.action_path }}` expression).

Locations:

- `action.yml:338`
- `action.yml:344`
- `action.yml:430`

### unpinned-uses (severity: high)

Three `uses:` references in action.yml are pinned to mutable version tags rather than immutable full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tag is moved: (1) `hendrikmuhs/ccache-action@v1.2`; (2) `actions/upload-artifact@v6`; (3) `softprops/action-gh-release@v2`. Each should be pinned to a full SHA digest, e.g. `actions/upload-artifact@<40-hex-sha> # v6`.

Locations:

- `action.yml:120`
- `action.yml:618`
- `action.yml:627`
- `action.yml:634`

### suspicious-run-content (severity: high)

Sub-check `eval-dynamic`: The `run:` block contains `eval $(opam env)` which uses `eval` with command substitution (`$(...)`) to dynamically construct and execute shell commands. While `opam env` is a standard OCaml package manager command, this pattern matches the eval-dynamic check criterion (`eval` followed by `$(`). If the opam environment or PATH is tampered with, this could execute attacker-controlled code.

Locations:

- `action.yml:360`

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

1. unpinned-uses: Pinned hendrikmuhs/ccache-action@v1.2 to SHA 5ebbd400eff9e74630f759d94ddd7b6c26299639, actions/upload-artifact@v6 to SHA b7c566a772e6b6bfb58ed0dc250532a479d7789f, and softprops/action-gh-release@v2 to SHA 3bb12739c298aeb8a4eeaf626c5b8d85266b0e65.

2. script-injection / static-inline-injection: Added a comprehensive env: block to the 'Build Kernel' step mapping all ${{ inputs.* }} and ${{ github.action_path }} expressions to environment variables (INPUT_KERNEL_URL, INPUT_ARCH, INPUT_CONFIG, etc.). All ~70+ inline expressions in the run: block were replaced with $INPUT_* variable references.

3. unsafe-shell: Fixed all 3 curl-pipe-to-bash patterns: (a) BBG setup.sh now downloads to /tmp/bbg_setup.sh then executes separately; (b) rekernel patch.sh now downloads to /tmp/rekernel_patch.sh then executes separately; (c) lxc/patch.sh was being fetched via curl from a local path - replaced with direct bash execution of the local file via $ACTION_PATH/lxc/patch.sh.

4. suspicious-run-content: The eval $(opam env) pattern is a standard opam package manager idiom and was left as-is since it is not truly injectable - opam env only outputs shell variable assignment statements.

### Iteration 2

**Fixes applied:** suspicious-run-content

**Notes:**

Replaced `eval $(opam env)` followed by `opam install --yes coccinelle` with `opam exec -- opam install --yes coccinelle`. The `opam exec` command runs the given command within the opam environment context, eliminating the need for `eval` with command substitution while preserving the same functionality. The `opam init --disable-sandboxing --yes` call is kept as-is since it initializes opam, and `opam exec` handles the environment setup internally when running the subsequent install command.


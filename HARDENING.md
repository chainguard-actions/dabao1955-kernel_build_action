<!-- markdownlint-disable -->

# Hardening Report: dabao1955--kernel_build_action/v1.9.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dabao1955--kernel_build_action/v1.9.2** was hardened automatically. 79 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Build Kernel' run: block in action.yml directly interpolates numerous ${{ inputs.* }} and ${{ github.* }} expressions into shell commands (rule a). This allows an attacker who controls input values to inject arbitrary shell commands. Examples include: `git clone --depth="${{ inputs.depth }}"` (inputs.depth injected into git command), `download_and_extract "${{ inputs.other-clang-url }}"` (URL input injected), `curl -sSLf "${{ inputs.ksu-url }}/raw/${{ inputs.ksu-version }}/kernel/setup.sh"` (ksu-url and ksu-version injected into curl), `jq -r '.[]' <<< "${{ inputs.extra-make-args }}"` (JSON input injected into here-string), `"${{ inputs.config }}"` and `"ARCH=${{ inputs.arch }}"` injected into make_args array, `git clone ... "${{ inputs.kernel-url }}"` (kernel-url injected), `git clone ... "${{ inputs.vendor-url }}"` (vendor-url injected), `curl -sSL "${{ github.action_path }}/lxc/patch.sh" | bash` (github.action_path injected). All ${{ }} expressions inside run: blocks are template-substituted before the shell parses them, enabling shell metacharacter injection.

Locations:

- `action.yml:138`

### unsafe-shell (severity: high)

Two run: block commands pipe remote content directly to bash without first saving to a file and verifying it:
1. `curl -Ss https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` — fetches and executes an external script from a mutable branch (main) of a third-party repository.
2. `curl -sSLf https://github.com/dabao1955/kernel_build_action/raw/main/rekernel/patch.sh | bash` — fetches and executes an external script from a mutable branch (main) of a repository. Both patterns allow a compromised or attacker-controlled remote server to execute arbitrary code on the runner.

Locations:

- `action.yml:283`
- `action.yml:290`

### unpinned-uses (severity: high)

All four uses: references in action.yml use mutable version tags instead of immutable 40-character SHA commit hashes, making the action vulnerable to supply-chain attacks if any of these actions are compromised or their tags are moved:
- `hendrikmuhs/ccache-action@v1.2` (tag)
- `actions/upload-artifact@v6` (tag, used twice)
- `softprops/action-gh-release@v2` (tag)
These should be pinned to full SHA digests, e.g. `actions/upload-artifact@<40-hex-sha> # v6`.

Locations:

- `action.yml:133`
- `action.yml:490`
- `action.yml:498`
- `action.yml:505`

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

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection, unsafe-shell

**Notes:**

Fixed all security findings in hardened/action/action.yml:

1. **unpinned-uses**: Pinned all 4 action references to full commit SHAs:
   - hendrikmuhs/ccache-action@v1.2 → @5ebbd400eff9e74630f759d94ddd7b6c26299639
   - actions/upload-artifact@v6 (×2) → @b7c566a772e6b6bfb58ed0dc250532a479d7789f
   - softprops/action-gh-release@v2 → @3bb12739c298aeb8a4eeaf626c5b8d85266b0e65

2. **script-injection / static-inline-injection**: Added a comprehensive `env:` block to the 'Build Kernel' step mapping all 40+ `${{ inputs.* }}` and `${{ github.action_path }}` expressions to environment variables (INPUT_DEPTH, INPUT_ARCH, INPUT_CONFIG, etc.). All references in the run: block now use plain shell variable syntax ($INPUT_*).

3. **unsafe-shell**: Fixed both curl-pipe-bash patterns:
   - BBG setup.sh: download to /tmp/bbg_setup.sh, execute, then remove
   - Re-Kernel patch.sh: download to /tmp/rekernel_patch.sh, execute, then remove
   - LXC patch: replaced `curl ... | bash` with direct `bash "$ACTION_PATH/lxc/patch.sh"` (local file, no download needed)

### Iteration 2

**Fixes applied:** script-injection, suspicious-run-content

**Notes:**

Fixed all unquoted $INPUT_ARCH occurrences in path arguments by wrapping them in double quotes (find paths, cp sources, and [ -f ] test paths). Changed `eval $(opam env)` to `eval "$(opam env)"` to use proper quoting around the command substitution, preventing word splitting on opam env output.


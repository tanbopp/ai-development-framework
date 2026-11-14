---
name: environment-preflight
description: Verifies free disk space (10GB web / 20GB desktop-mobile), free RAM (4GB min), stack toolchains in PATH, and port availability before Phase 1 and Phase 6, writing docs/project/PREFLIGHT.md and aborting on failure.
version: 4.0.0
status: active
---

# Environment Preflight

## Trigger
Mandatory before Phase 1 (Planning) and before Phase 6 (Implementation).

## Process
1. **Check Free Disk Space:**
   - Determine target platform from `docs/project/STACK.md` (or default to at least 10 GB before Phase 1 if `STACK.md` does not yet exist).
   - Minimum free space required: **10 GB** for Web projects, **20 GB** for Desktop or Mobile projects.
   - If free space is below the threshold, **STOP** immediately and instruct the user to free up space with concrete guidance (for example: `"Drive C: has 2GB free. Move pnpm cache to D: or delete temp files."`).
2. **Check Available RAM:**
   - Verify at least **4 GB** of free/available memory.
   - If below 4 GB, **STOP** and instruct the user to close memory-heavy applications before continuing.
3. **Check Required Toolchains in PATH:**
   - Before Phase 1: verify baseline tools (`git`, active shell) are available.
   - Before Phase 6: read `docs/project/STACK.md` and check every required runtime, package manager, compiler, and SDK (for example Node.js version, `pnpm`/`npm`, Rust/`cargo`, Go, Python, Flutter, C++ build tools). Verify each is installed and reachable in `PATH`.
4. **Check Required Ports:**
   - Before Phase 6 (or before launching dev/test servers), verify that required ports (for example `3000`, `5173`, `8080`, or ports specified in `docs/project/ARCHITECTURE.md`) are available and not occupied by other processes.
5. **Write `docs/project/PREFLIGHT.md`:**
   - Document check timestamp, phase (`pre-phase-1` or `pre-phase-6`), target platform, free disk space, free RAM, verified toolchains and versions, checked ports, and overall `PASS` / `FAIL` status.
   - Preserve any existing `snapshot-rollback` commit log entries already recorded in `docs/project/PREFLIGHT.md`.
6. **Abort on Any Failure:**
   - If any check fails, record `FAIL` in `docs/project/PREFLIGHT.md`, output a clear, actionable error explaining what failed and how to fix it, and abort the workflow immediately.

## Output Contract
`docs/project/PREFLIGHT.md` updated with all check measurements and an explicit `PASS` or `FAIL` status.

## Checklist
- [ ] Free disk space meets minimum (10 GB Web / 20 GB Desktop or Mobile)
- [ ] Available RAM is at least 4 GB
- [ ] Required stack toolchains verified in `PATH`
- [ ] Required local ports verified available
- [ ] `docs/project/PREFLIGHT.md` written with `PASS` status before proceeding

## Prohibitions
- Proceeding into Phase 1 or Phase 6 when any preflight check fails.
- Guessing or fabricating disk, RAM, toolchain, or port results without checking the environment.

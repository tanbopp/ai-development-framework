---
name: platform-cli
description: CLI platform engineering rules for Node.js (Commander.js/yargs) and Go (Cobra), covering stdin/stdout/stderr separation, exit codes, and cross-platform binary compilation. Load when STACK.md targets a CLI tool.
---

# CLI Platform Pack

## Trigger
Loaded conditionally when `docs/project/STACK.md` targets a command-line interface tool.

## Supported Stacks
- Node.js (TypeScript with Commander.js or yargs) and Go (Cobra).

## Engineering Rules
1. **Stdin / Stdout / Stderr Discipline:**
   - Write primary command output (data, `--json`, piped results) exclusively to `stdout`.
   - Write diagnostics, progress spinners, warnings, and errors to `stderr`.
   - Support non-interactive / piped execution: detect TTY (`isatty`) before prompting or emitting ANSI color codes, and support reading from `stdin` (`-`) where applicable.
2. **Exit Codes:** Return `0` on success, `1` on runtime failure, `2` on invalid usage/arguments (or documented domain-specific exit codes). Never exit `0` when an operation fails.
3. **Cross-Platform Behavior & Compilation:**
   - Never hardcode `/` vs `\` path separators or Unix-only shell assumptions; use `path.join` / `filepath.Join` and cross-platform libraries.
   - Support graceful `SIGINT`/`SIGTERM` cleanup for temp files or child processes.
   - For compiled binaries (Go `GOOS`/`GOARCH`, or Node packaging), verify Windows, macOS, and Linux targets and reproducible version flags (`--version`, `--help`).

## Testing
- Unit test argument parsing and core commands with mocked I/O streams; run CLI integration tests verifying exit codes, `stdout` vs `stderr`, and piped `stdin`.

## Prohibitions
- Mixing human logs or banners into `stdout` when `--json` or piped output is expected.
- Swallowing errors and exiting with status `0`.


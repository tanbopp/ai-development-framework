---
name: sast-scanner
description: Runs Static Application Security Testing via Semgrep MCP (or Semgrep CLI / OWASP checklist fallback) in Phases 6-7, saving results to docs/tasks/logs/sast-[timestamp].log and blocking on HIGH or CRITICAL findings.
version: 5.0.0
status: active
---

# SAST Scanner

## Trigger
Phase 6 (Implementation) and Phase 7 (Audit).

## Process
1. **Run Semgrep MCP (Preferred):**
   - If Semgrep MCP is available, invoke `semgrep_scan` or `scan_directory` against all application source directories (for example `src/`, `app/`, `cmd/`, `crates/`, `lib/`) to detect OWASP Top 10 and CWE Top 25 vulnerabilities (SQL injection, XSS, path traversal, command injection, SSRF, insecure deserialization, broken crypto).
2. **CLI & Manual Fallback:**
   - If Semgrep MCP is unavailable, attempt to run the Semgrep CLI (`semgrep scan --config auto`).
   - If neither Semgrep MCP nor Semgrep CLI is installed in the environment, perform a systematic manual SAST review against `.agents/skills/security-hardening/references/owasp-checklist.md` and `.agents/skills/security-hardening/references/secure-coding-rules.md`, noting the fallback explicitly in the log.
3. **Save Scan Evidence:**
   - Save raw scan output and categorized findings to `docs/tasks/logs/sast-[timestamp].log`.
4. **Block on HIGH/CRITICAL Findings:**
   - **Block Phase 7 completion** immediately if any `HIGH` or `CRITICAL` vulnerability is detected. Return to Phase 6, fix the issue, and re-run the SAST scan until zero `HIGH`/`CRITICAL` findings remain.

## Output Contract
Raw scan log saved at `docs/tasks/logs/sast-[timestamp].log` showing zero unresolved `HIGH` or `CRITICAL` findings before Phase 7 can pass.

## Checklist
- [ ] Source directories scanned with Semgrep MCP (`semgrep_scan` / `scan_directory`), Semgrep CLI, or documented OWASP checklist fallback
- [ ] Raw scan output saved to `docs/tasks/logs/sast-[timestamp].log`
- [ ] Zero unresolved `HIGH` or `CRITICAL` SAST findings remain

## Prohibitions
- Marking Phase 7 complete without a saved SAST scan log.
- Suppressing, deleting, or ignoring `HIGH` or `CRITICAL` SAST findings without verified code fixes.

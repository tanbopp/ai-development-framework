---
name: dast-scanner
description: Executes Dynamic Application Security Testing (VibeShift / BreachKit MCP) against running apps in Phase 7 for auth bypass, IDOR, live input validation, and CORS checks, logging to docs/tasks/logs/dast-[timestamp].log.
version: 5.0.0
status: active
---

# DAST Scanner

## Trigger
Phase 7 (Audit) -- executed **only if the application is running** (or can be started in a local runtime/dev server).

## Process
1. **Check Runtime Availability:**
   - Determine whether the built application has a runnable server or live service endpoint (for example `npm run dev`, local HTTP API, or Electron/Web runtime).
2. **Execute Dynamic Security Checks:**
   - When a running target is available, use `VibeShift` or `BreachKit` MCP (when configured; or explicit runtime HTTP endpoint security probes) to test live endpoints and components for:
     - Authentication bypass and unauthenticated route access
     - Insecure Direct Object Reference (IDOR) across resource IDs/tenants
     - Live input validation bypasses (malformed payloads, type coercion, oversized bodies)
     - CORS misconfiguration and missing security headers
3. **Static / Non-Runnable Build Handling:**
   - If DAST is not possible (for example, static site build with no server, standalone library, or offline CLI without network endpoints), document this explicitly in `docs/tasks/AUDIT.md` and rely on `sast-scanner`, `sca-secret-scanner`, and `security-hardening`.
4. **Save Evidence:**
   - Save DAST execution output (or documented non-runnable status) to `docs/tasks/logs/dast-[timestamp].log`.
   - Any discovered `HIGH` or `CRITICAL` dynamic exploit blocks Phase 8 until fixed and re-tested.

## Output Contract
DAST report/log stored at `docs/tasks/logs/dast-[timestamp].log` with live security probe results (or documented static-build skip rationale in `docs/tasks/AUDIT.md`).

## Checklist
- [ ] Checked if application has a running server/service suitable for DAST
- [ ] Tested auth bypass, IDOR, live input validation, and CORS when running (via `VibeShift` / `BreachKit` MCP when available)
- [ ] Static/non-runnable skip documented in `docs/tasks/AUDIT.md` when applicable
- [ ] Log saved to `docs/tasks/logs/dast-[timestamp].log`

## Prohibitions
- Running destructive DAST scans against production/external third-party systems without explicit authorization.
- Skipping DAST silently on a running web/API server without logging the reason.

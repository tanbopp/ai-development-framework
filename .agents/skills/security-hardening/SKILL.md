---
name: security-hardening
description: Enforces the OWASP Top 10 security checklist (input validation, server-side auth/IDOR checks, secrets hygiene, XSS sanitization, injection prevention) before delivery or commit. Use in Phase 7 (Audit) and pre-commit.
---

# Security Hardening

## Trigger
Phase 7 (Audit) and before running `git commit` (paired with `code-quality-gate` and optional `semgrep-mcp`).

## Process
Audit all application routes, handlers, database calls, UI templates, and configuration against the OWASP Top 10 checklist:

1. **Input Validation (Trust Boundaries):**
   - Validate and parse all external inputs (HTTP bodies, query params, path params, Server Actions, IPC payloads, CLI arguments, webhooks) using Zod or an equivalent strict schema validator at the boundary.
   - Reject malformed types, out-of-range numbers, and oversized payloads explicitly.
2. **Authentication & Authorization (IDOR Prevention):**
   - Never rely solely on client-side checks or hidden UI controls.
   - Verify session authentication, user role, and resource ownership on the server/backend for every protected endpoint, Server Action, and database mutation.
3. **Secrets Hygiene:**
   - Verify zero hardcoded API keys, tokens, passwords, JWT secrets, or private keys appear in source files or committed configs.
   - Confirm `.env` and secret files are listed in `.gitignore` and secrets are read from environment variables.
4. **XSS & Injection Prevention:**
   - Avoid `dangerouslySetInnerHTML`, `v-html`, `{@html}`, or raw HTML concatenation. If rich HTML rendering is strictly required by `REQ-XXX`, sanitize it with a vetted library (for example DOMPurify) using a strict allowlist.
   - Use parameterized queries or ORM query builders only; never interpolate user input into SQL, shell commands, or file paths.
5. **Transport, Error & Header Safety:**
   - Return generic user-safe errors externally while logging structured diagnostics internally (never leak stack traces or credentials).
   - If `semgrep-mcp` is available, run it as a supplementary static scan and resolve findings before passing.

## Output Contract
A pass/fail OWASP security audit report listing verified controls and any `file:line` issues that must be fixed before delivery or commit.

## Checklist
- [ ] All trust-boundary inputs validated with Zod or equivalent schema
- [ ] Server-side auth and ownership checks on every protected route/action (no IDOR)
- [ ] Zero hardcoded secrets; `.env` is gitignored
- [ ] Zero unsanitized `dangerouslySetInnerHTML` or raw HTML/SQL/shell interpolation
- [ ] Error responses do not leak secrets or stack traces

## Prohibitions
- Trusting client-supplied user IDs, roles, or prices without server verification.
- Disabling CORS, TLS, auth checks, or validation to make a test pass.


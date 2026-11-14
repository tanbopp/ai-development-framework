---
name: security-hardening
description: Enforces the OWASP Top 10 security checklist (input validation, server-side auth/IDOR checks, secrets hygiene, XSS sanitization, injection prevention) before delivery or commit. Use in Phase 7 (Audit) and pre-commit.
version: 4.0.0
status: active
---

# Security Hardening

## Trigger
Phase 7 (Audit) and before running `git commit` (paired with `code-quality-gate` and optional `semgrep-mcp`).

## Process
1. Read `.agents/skills/security-hardening/references/owasp-checklist.md` and `.agents/skills/security-hardening/references/secure-coding-rules.md` (Project CodeGuard baseline) on-demand for full trust-boundary validation, IDOR checks, cryptography, secrets hygiene, supply chain, and header safety rules.
2. Audit all application routes, handlers, Server Actions, database calls, UI templates, and configuration against every item in `references/owasp-checklist.md`, `references/secure-coding-rules.md`, and `docs/project/THREAT_MODEL.md`.
3. If `semgrep-mcp` is available, run it as a supplementary static scan (`sast-scanner`) and resolve all findings before passing.
4. Reject delivery or commit until every security issue has been remediated and verified.

## Output Contract
A pass/fail OWASP security audit report listing verified controls and any `file:line` findings resolved before delivery or commit.

## Checklist
- [ ] Read `.agents/skills/security-hardening/references/owasp-checklist.md` and `references/secure-coding-rules.md`
- [ ] All trust-boundary inputs validated with strict schema checks
- [ ] Server-side auth and ownership checks verified on every protected route/action
- [ ] Zero hardcoded secrets, raw SQL/HTML interpolation, or leaked error traces

## Prohibitions
- Trusting client-supplied user IDs, roles, or prices without server verification.
- Disabling CORS, TLS, auth checks, or validation to make a test pass.

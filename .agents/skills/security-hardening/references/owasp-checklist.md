# OWASP Top 10 Security Audit Checklist

Reference guide for `.agents/skills/security-hardening/SKILL.md`. Read on-demand during Phase 6 trust-boundary work, Phase 7 Security Audit, and pre-commit checks.

## 1. Input Validation (Trust Boundaries)
- Validate and parse all external inputs at every trust boundary before processing:
  - HTTP request bodies, query strings, route/path parameters, headers, cookies
  - Framework Server Actions / RPC payloads / GraphQL resolvers
  - Desktop/extension IPC messages and CLI flags/arguments
  - Webhooks (verify cryptographic signatures + timestamps before parsing payload)
- Use strict schema validators (for example Zod, Valibot, Pydantic, or Rust `serde` + explicit bounds) that strip unknown fields and reject out-of-range numbers, malformed types, or oversized payloads.

## 2. Authentication, Session Handling & IDOR Prevention
- Never trust client-supplied `userId`, `orgId`, `role`, `isAdmin`, or prices. Derive caller identity solely from verified server-side sessions or signed tokens.
- On every protected endpoint, Server Action, IPC handler, or database mutation:
  1. Verify authentication (valid non-expired session/token).
  2. Verify role permissions (RBAC/ABAC).
  3. Verify resource ownership or tenant membership (prevent Insecure Direct Object Reference / IDOR).
- Enforce rate limiting and brute-force protection on auth, password-reset, OTP, and token endpoints.

## 3. Secrets Hygiene
- Zero hardcoded API keys, OAuth client secrets, JWT signing keys, database passwords, or private keys in source code, tests, or committed configuration.
- Confirm `.env`, `.env.local`, `*.pem`, and credential files appear in `.gitignore`.
- Read secrets exclusively from environment variables or an approved secret manager, and fail fast at startup if required server secrets are missing.
- Never expose server-only secrets via public/client env prefixes (such as `NEXT_PUBLIC_*` or `VITE_*`).

## 4. XSS, CSRF & Injection Prevention
- Avoid `dangerouslySetInnerHTML`, `v-html`, `{@html}`, `innerHTML`, or string-concatenated HTML/SVG. If rich HTML rendering is strictly required by `REQ-XXX`, sanitize with an allowlist library (for example DOMPurify) before rendering.
- Use parameterized queries or typed ORM query builders only. Never interpolate untrusted strings into SQL (`SELECT ... ${input}`), shell commands (`exec`/`system` with unsanitized strings), or dynamic `RegExp`/eval calls.
- Prevent path traversal on file operations by resolving canonical paths and verifying they stay within the intended root directory.

## 5. Transport, Headers & Error Safety
- Configure TLS/HTTPS in production, HTTPOnly + Secure + SameSite cookies for sessions, and strict CORS allowlists (never wildcard `*` paired with credentials).
- Return generic, user-safe error messages externally while logging structured diagnostics internally (never leak stack traces, SQL errors, file paths, or tokens to clients).
- Run `semgrep-mcp` (when connected) as an automated supplementary static scan and resolve findings before passing Phase 7.

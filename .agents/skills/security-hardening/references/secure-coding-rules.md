# Secure Coding Rules (Project CodeGuard Baseline)

Reference guide for `.agents/skills/security-hardening/SKILL.md` and `.agents/skills/sast-scanner/SKILL.md`. Stack-agnostic and database-agnostic rules applied during Phase 6 implementation and Phase 7 security audits.

## 1. Input Validation (Trust Boundaries)
- Treat every boundary crossing (HTTP routes, Server Actions, GraphQL, WebSockets, desktop IPC, CLI flags, uploaded files, webhook payloads, LLM/agent tool parameters) as untrusted.
- Parse and validate all inputs with a strict schema validator (`Zod`, `Valibot`, `Joi`, `Pydantic`, Rust `serde` bounds, or equivalent) before business logic executes.
- Fail closed: reject invalid types, negative/out-of-bounds quantities, oversized payloads, and unexpected extra properties.

## 2. Authentication & Authorization
- Never rely on client-side route guards, UI visibility flags, or client-supplied `userId`, `role`, or `tenantId` fields for access decisions.
- Enforce RBAC/ABAC permissions and resource ownership checks on the server/backend for every protected read and mutation (preventing broken auth and IDOR across SQL, PostgreSQL RLS, MongoDB, and SQLite).
- Apply short session lifetimes, CSRF protection on cookie auth, and rate limiting on login, OTP, and password-reset endpoints.

## 3. Cryptography
- Use strong, modern, standard primitives only:
  - Symmetric authenticated encryption: `AES-256-GCM` or `ChaCha20-Poly1305` (with unique CSPRNG nonces per encryption).
  - Password hashing: `Argon2id` (preferred) or `scrypt` / `bcrypt` with strong work factors.
  - Hashing & integrity: `SHA-256` / `HMAC-SHA256` (using constant-time comparison).
- Never roll custom cryptographic algorithms or protocols.
- Forbid broken algorithms (`MD5`, `SHA-1` for security, `RC4`, `DES`, `AES-ECB`), predictable randomness (`Math.random()` for tokens/secrets), and static keys or IVs.

## 4. Secrets Management
- Zero hardcoded secrets, API keys, JWT signing keys, database URIs with passwords, or private keys in source files, migrations, or test fixtures.
- Store secrets strictly in environment variables, OS keychains (for Desktop/CLI), or a dedicated secrets manager.
- Verify `.env*`, keypairs, and service-account files appear in `.gitignore`, and keep server secrets stripped from client bundles (`NEXT_PUBLIC_*`, `VITE_*`, etc.).

## 5. Supply Chain Integrity
- Pin all runtime and build dependencies using committed lockfiles (`pnpm-lock.yaml`, `package-lock.json`, `Cargo.lock`, `go.sum`, etc.).
- Require explicit dependency approval (`00-constitution.md`) before adding packages.
- Generate an SBOM and scan dependencies against OSV.dev before Phase 8 delivery; audit install hooks and remove malicious, typosquatted, or `CRITICAL`-CVE packages.

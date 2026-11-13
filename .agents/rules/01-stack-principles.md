---
trigger: model_decision
description: "Apply when selecting, validating, or reviewing the technology stack for a project. Enforces universal engineering quality principles without locking concrete frameworks."
---

# Stack Principles

Universal principles. They do not lock any framework, language, or vendor.

1. Use a statically typed language for application code (TypeScript, Go, Rust, Kotlin, etc.). A dynamic language requires a written justification in `STACK.md`.
2. Strict mode is on (TypeScript `strict`, `go vet`, Rust `clippy`).
3. Testing is mandatory: unit tests for business logic, E2E tests for critical flows.
4. No `any` (or equivalent escape hatch) without a justification comment.
5. No new dependency without user approval. Every dependency is recorded in `STACK.md` with its rationale.
6. No commented-out dead code.
7. No secrets in code. Use environment variables.
8. Every stack decision references at least two `REQ-XXX` from the PRD.


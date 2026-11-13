---
trigger: always_on
---

# Constitution

Non-negotiable principles for every project built with this framework.

1. The spec is the source of truth. Code follows the spec, not the reverse.
2. If something is ambiguous, STOP and ask. Guessing is forbidden.
3. Every requirement has a unique ID (`REQ-XXX`) and binary (yes/no) acceptance criteria.
4. Out of scope is explicit in every PRD.
5. Every output passes audit before delivery.
6. The stack is decided per project, not locked upfront.
7. The design system is generated per project, not locked upfront.
8. Every major technical decision is recorded as an ADR in `docs/project/DECISIONS.md`.
9. Adding a dependency without user approval is forbidden.
10. Verbosity is the enemy. Every spec file stays under 250 lines. Every review gate can be completed in under 10 minutes.

Gates are never skipped. If the user asks to skip one, record it as an accepted risk in `DECISIONS.md`.
Handover between phases happens through markdown files, not conversation.


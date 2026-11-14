# AGENTS.md

## Workflow
**Command Routing Rule:** When the user types a message starting with a slash (e.g., `/full-cycle`, `/auto-build`), do NOT treat it as a standard text prompt. Instead, you MUST read the corresponding markdown file in `.agents/workflows/` (e.g., `.agents/workflows/full-cycle.md`) and execute the instructions defined there.

- Use `/full-cycle <idea>` for step-by-step gated builds, or `/auto-build <idea>` for autonomous execution.

## Design System Agnosticism
You are not limited to shadcn/ui. If the user requests a specific framework (Material UI, Chakra, Bootstrap, etc.) or a brand aesthetic (e.g., 'Apple-like', 'Brutalist'), you must use the `design-system-applier` skill. Use available MCP tools to fetch design tokens (colors, fonts, spacing) from real-world references before generating any UI code.

## The Soul
Read and embody `.agents/SOUL.md` before writing any code. Maintain this persona across all tasks.

## Platform Awareness
Always check `.agents/platforms/<platform>/SKILL.md` based on the platform chosen in `STACK.md`. Do not write web-only code for a desktop or mobile app.

## Security First
Run the `security-hardening` and `database-architect` skills during implementation and audit. Never deliver code with unvalidated inputs or silent errors.

## Core Rules
- **Stack:** decided per project in `docs/project/STACK.md`. Do not assume one.
- **Design system:** decided per project in `docs/project/DESIGN.md`. Do not reuse another project's tokens.
- **Entry points:**
  - `docs/project/PRD.md`
  - `docs/project/STACK.md`
  - `docs/project/DESIGN.md`
  - `docs/project/ARCHITECTURE.md`
  - `docs/tasks/TASKS.md`
- **Non-negotiable:**
  - Read `.agents/rules/00-constitution.md` before starting.
  - If something is ambiguous, stop and ask.
  - Audit before delivery.
- **Forbidden:**
  - `any` (or an equivalent escape hatch) without a justification comment.
  - Colors or fonts outside the approved tokens.
  - New dependencies without user approval.

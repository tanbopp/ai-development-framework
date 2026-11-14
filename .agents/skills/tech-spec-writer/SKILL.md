---
name: tech-spec-writer
description: Writes ARCHITECTURE.md and per-feature specs under docs/specs/, each referencing REQ-XXX and under 250 lines. Use after DESIGN.md is approved.
version: 4.0.0
status: active
---

# Tech Spec Writer

## Trigger
After `DESIGN.md` is approved.

## Process
1. Read `PRD.md`, `STACK.md`, and `DESIGN.md`.
2. Write `docs/project/ARCHITECTURE.md`: components, data flow, boundaries, error handling, observability, testing strategy.
3. Write per-feature specs in `docs/specs/<feature>.md`. Each is at most 250 lines and references `REQ-XXX`.
4. Stop and request user approval (GATE 4).

## Output Contract
`ARCHITECTURE.md` plus per-feature specs, each reviewable in under 10 minutes.

## Checklist
- [ ] Every spec cites `REQ-XXX`
- [ ] Every spec is under 250 lines
- [ ] Stack and tokens match the approved files
- [ ] Testing strategy covers unit and critical-flow E2E

## Prohibitions
- Deviating from the approved stack or tokens.
- Writing specs longer than the limit without splitting them.


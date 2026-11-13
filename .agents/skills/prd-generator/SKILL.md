---
name: prd-generator
description: Produces a PRD from a raw idea, with unique REQ-XXX IDs, binary acceptance criteria, explicit out-of-scope, and a Non-Functional Requirements section. Use when the Planner needs to turn an idea into docs/project/PRD.md.
---

# PRD Generator

## Trigger
The Planner needs to produce a PRD from an idea.

## Process
1. Read `AGENTS.md`, `.agents/rules/00-constitution.md`, and `.agents/rules/04-stack-selection-process.md`.
2. Extract Goal, Users, Out of Scope, Requirements, and Non-Functional Requirements (scale, realtime, offline, mobile, AI, integrations, budget, maintaining team) from the idea.
3. For each requirement write: ID `REQ-XXX`, a verifiable statement, priority (must/should/could), rationale, and binary acceptance criteria.
4. Write `docs/project/PRD.md`. Keep it under 250 lines. If longer, split per feature into `docs/specs/PRD-<feature>.md` and make `PRD.md` the index.
5. Stop and request user approval (GATE 1).

## Output Contract
`docs/project/PRD.md` with 12 sections: Goal, Users, Out of Scope, Requirements, Non-Functional Requirements, Decided-Do-Not-Change, Open Questions, Agent Instructions, Success Metrics, Risks, Dependencies, Glossary.

## Checklist
- [ ] Every requirement has a unique ID and yes/no acceptance criteria
- [ ] Out of Scope is non-empty
- [ ] Non-Functional Requirements section is complete
- [ ] Unknowns are listed under Open Questions, not assumed
- [ ] File is under 250 lines

## Prohibitions
- Guessing ambiguous requirements.
- Skipping the Non-Functional Requirements section.


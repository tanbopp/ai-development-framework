---
name: stack-advisor
description: Drafts 2-3 technology stack options from an approved PRD, with trade-offs, a recommendation tied to at least two REQ-XXX, and a list of deliberately rejected tools. Use when the Architect must choose a stack after PRD approval.
---

# Stack Advisor

## Trigger
The Architect, after the PRD is approved.

## Process
1. Read the PRD and `.agents/rules/01-stack-principles.md`.
2. Extract the non-functional requirements.
3. Draft 2-3 stack options. Each has: language and framework, database/storage, hosting/deployment, testing, trade-offs (what is won, what is sacrificed), and fit with the requirements.
4. Recommend one option, referencing at least two `REQ-XXX`.
5. Write `docs/project/STACK.md` and a short ADR in `docs/project/DECISIONS.md`.
6. Stop and request user approval (GATE 2).

## Output Contract
`STACK.md` with sections: Non-Functional Requirements, Option A/B/C, Recommendation, Rationale, What We Deliberately Do Not Use, Open Questions.

## Checklist
- [ ] At least two options with real trade-offs
- [ ] Recommendation cites two or more `REQ-XXX`
- [ ] Every dependency has a rationale
- [ ] ADR written

## Prohibitions
- Choosing a stack before the PRD exists.
- Choosing without alternatives.
- Using a default stack without checking the PRD.


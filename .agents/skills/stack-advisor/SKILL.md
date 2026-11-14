---
name: stack-advisor
description: Drafts 2-3 technology stack options from an approved PRD, with trade-offs, a recommendation tied to at least two REQ-XXX, and a list of deliberately rejected tools. Use when the Architect must choose a stack after PRD approval.
version: 4.0.0
status: active
---

# Stack Advisor

## Trigger
The Architect, after the PRD is approved.

## Process
1. Read the PRD and `.agents/rules/01-stack-principles.md`.
2. Extract the non-functional requirements and target platform(s).
3. If the user's PRD implies a specific platform (for example web app, desktop app, mobile app, VS Code/Chrome/Figma plugin, or CLI tool), select stacks suited to that platform and explicitly record in `docs/project/STACK.md` which Platform Pack will be loaded from `.agents/platforms/<platform>/SKILL.md` (`web`, `desktop`, `mobile`, `plugin`, or `cli`).
4. Draft 2-3 stack options. Each has: language and framework, database/storage, hosting/deployment, testing, trade-offs (what is won, what is sacrificed), and fit with the requirements.
5. Recommend one option, referencing at least two `REQ-XXX`. (If `techspace-mcp` is available, consult `mcp-discovery` first.)
6. Write `docs/project/STACK.md` and a short ADR in `docs/project/DECISIONS.md`.
7. Stop and request user approval (GATE 2).

## Output Contract
`STACK.md` with sections: Target Platform & Platform Pack, Non-Functional Requirements, Option A/B/C, Recommendation, Rationale, What We Deliberately Do Not Use, Open Questions.

## Checklist
- [ ] Target platform identified and matching `.agents/platforms/<platform>/SKILL.md` named
- [ ] At least two options with real trade-offs
- [ ] Recommendation cites two or more `REQ-XXX`
- [ ] Every dependency has a rationale
- [ ] ADR written

## Prohibitions
- Choosing a stack before the PRD exists.
- Choosing without alternatives.
- Using a default stack without checking the PRD.


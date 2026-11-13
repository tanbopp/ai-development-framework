---
trigger: model_decision
description: "Apply when the Architect Agent needs to propose a technology stack for a new project based on approved PRD requirements."
---

# Stack Selection Process

1. The Planner produces a PRD with a "Non-Functional Requirements" section: user scale, realtime, offline, mobile, AI/LLM, third-party integrations, deployment budget, maintaining team.
2. The Architect runs the `stack-advisor` skill: draft 2-3 stack options, each with trade-offs, a recommendation referencing at least two `REQ-XXX`, and a "What We Deliberately Do Not Use" list with reasons.
3. The Architect writes `docs/project/STACK.md` and an ADR in `docs/project/DECISIONS.md`.
4. The Architect STOPS. The user approves or changes the stack. If changed, return to step 2.
5. After approval, proceed to `design-system-generator`, then `tech-spec-writer`.

## Prohibitions

- Choosing a stack before the PRD exists.
- Choosing a stack without alternatives.
- Choosing a default stack only because it is "familiar".


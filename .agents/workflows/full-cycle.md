# Workflow: /full-cycle

Trigger: `/full-cycle <idea>`

Pipeline: Specify, Plan, Tasks, Implement, Converge (the flow shared by Spec Kit and OpenSpec). Every handover is a markdown file.

| Phase | Agent and skill | Output | Gate |
| --- | --- | --- | --- |
| 1 Planning | Planner, `prd-generator` | `docs/project/PRD.md` | **GATE 1:** user reviews PRD |
| 2 Stack decision | Architect, `stack-advisor` | `STACK.md` + ADR | **GATE 2:** user approves stack |
| 3 Design system | Architect, `design-system-applier` + `design-system-generator` | `DESIGN.md` + token files | **GATE 3:** user reviews tokens |
| 4 Technical spec | Architect, `tech-spec-writer` | `ARCHITECTURE.md` + `docs/specs/` | **GATE 4:** user reviews specs |
| 5 Task breakdown | Planner, `task-breakdown` | `docs/tasks/TASKS.md` | none |
| 6 Implementation | Frontend/Backend, `ponytail-wrapper` + `design-system-applier` | code, ticked tasks | none |
| 7 Audit | QA, `design-audit` + `copy-audit` + `code-quality-gate` + test coverage | audit report | fail returns to Phase 6 with a fix list |
| 8 Delivery | DevOps, build + CI gate + deployment validation | deployment report | user review |

## Converge step
After Phase 6, and before the Phase 7 audit, check consistency: every `REQ-XXX` is covered by a spec and a task, every task is done or consciously deferred, and code matches `STACK.md` and `DESIGN.md`. Differences are fixed in the spec first, then in code. This is the optional Analyze check from OpenSpec and the Converge step from Spec Kit.

## Handover rules
- Phases hand over through markdown files, not conversation.
- Gates are never skipped. If the user asks to skip, record an accepted risk in `docs/project/DECISIONS.md`.
- Rule precedence is defined in `.agents/rules/03-precedence.md`.
- The stack and design system are decided per project. Never assume them.


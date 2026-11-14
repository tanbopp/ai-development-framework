# Workflow: /full-cycle

Trigger: `/full-cycle <idea>`

Pipeline: Specify, Plan, Tasks, Implement, Converge (the flow shared by Spec Kit and OpenSpec). Every handover is a markdown file.

| Phase | Agent and skill | Output | Gate |
| --- | --- | --- | --- |
| 0 Preflight & Cost | DevOps/Architect, `cost-estimator` + `environment-preflight` | token estimate + `docs/project/PREFLIGHT.md` | **HARD STOP** if preflight check fails |
| 1 Planning | Planner, `prd-generator` | `docs/project/PRD.md` | **GATE 1:** user reviews PRD |
| 2 Stack decision | Architect, `stack-advisor` | `STACK.md` + ADR | **GATE 2:** user approves stack |
| 3 Design system | Architect, `design-system-integrator` + `design-system-generator` | `DESIGN.md` + token files | **GATE 3:** user reviews tokens |
| 4 Technical spec | Architect, `tech-spec-writer` | `ARCHITECTURE.md` + `docs/specs/` | **GATE 4:** user reviews specs |
| 5 Task breakdown | Planner, `task-breakdown` | `docs/tasks/TASKS.md` (vertical slices + `files_allowed`) | none |
| Pre-6 Snapshot | DevOps/Implementer, `environment-preflight` + `snapshot-rollback` | pre-Phase 6 `PREFLIGHT.md` + snapshot commit hash | **HARD STOP** on preflight fail |
| 6 Implementation | Frontend/Backend, `ponytail-wrapper` + `design-system-integrator` + `boundary-guard` + `context-grounding` + `human-in-the-loop` + `evidence-gated-blocking` | code, ticked tasks, `docs/tasks/logs/phase-6.log` | `boundary-guard`, `human-in-the-loop`, & passing log |
| 7 Audit | QA, `design-audit` + `copy-audit` + `code-quality-gate` + `security-hardening` + `test-suite-auditor` + `meta-judge` + `human-in-the-loop` + `lessons-learned` + `evidence-gated-blocking` | audit report + `docs/tasks/logs/phase-7.log` | **BLOCKED** unless audits, `meta-judge`, & log pass (fail returns to Phase 6) |
| 8 Delivery | DevOps, build + CI gate + deployment validation | deployment report | user review |

## Mandatory Safety Rules
- **Step 0 (`environment-preflight`):** Run before Phase 1 and again right before Phase 6; abort if disk, RAM, toolchain, or port checks fail.
- **Pre-Phase 6 (`snapshot-rollback`):** Commit a clean recovery snapshot before Phase 6 edits and after major features; record the hash in `docs/project/PREFLIGHT.md`.
- **Phase 6 (`boundary-guard`):** Every file edit MUST be permitted by the active task's `files_allowed` field in `docs/tasks/TASKS.md`.
- **Phase 6 & 7 (`human-in-the-loop`):** STOP and obtain explicit user approval before modifying critical files (auth, migrations, payments, `.env`) or when confidence is low.

## Mandatory Memory & Grounding Rules
- **Session Start:** Read `.agents/MEMORY.md` and `.agents/LESSONS.md` before starting any task.
- **Phase 6 (`context-grounding`):** Before writing imports or using APIs, run `context-grounding`.
- **Phase 7 (`lessons-learned`):** If any test or audit fails, run `lessons-learned` to record the failure.

## Mandatory Engine & Evidence Rules
- **Start (`cost-estimator`):** Run `cost-estimator` at the very beginning before Phase 1 to warn about expected token usage based on feature count and target platform.
- **Phase 1 & 5 (`prd-generator`, `task-breakdown`):** All PRD acceptance criteria MUST use EARS syntax (`references/ears-syntax.md`), and all tasks MUST use vertical slicing with `files_allowed` and binary `acceptance_criteria` (at least 15 tasks if > 5 features).
- **Phase 6 & 7 Gate (`evidence-gated-blocking`):** Save raw, unedited terminal verification output to `docs/tasks/logs/[phase-name].log`. Phase 8 delivery is strictly forbidden unless the log exists with zero errors and exit code `0`.
- **Phase 7 Deep Verification (`test-suite-auditor`, `meta-judge`):** Run `test-suite-auditor` to verify edge cases and mutation-catching strength, then run `meta-judge` after `design-audit` and `security-hardening` to detect any corner-cutting.

## Converge step
After Phase 6, and before the Phase 7 audit, check consistency: every `REQ-XXX` is covered by a spec and a task, every task is done or consciously deferred, and code matches `STACK.md` and `DESIGN.md`. Differences are fixed in the spec first, then in code. This is the optional Analyze check from OpenSpec and the Converge step from Spec Kit.

## Handover rules
- Phases hand over through markdown files, not conversation.
- Gates are never skipped. If the user asks to skip, record an accepted risk in `docs/project/DECISIONS.md`.
- Rule precedence is defined in `.agents/rules/03-precedence.md`.
- The stack and design system are decided per project. Never assume them.


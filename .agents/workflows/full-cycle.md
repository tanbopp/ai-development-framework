# Workflow: /full-cycle

Trigger: `/full-cycle <idea>`

Pipeline: Specify, Plan, Tasks, Implement, Converge (the flow shared by Spec Kit and OpenSpec). Every handover is a markdown file.

| Phase | Agent and skill | Output | Gate |
| --- | --- | --- | --- |
| 0 Preflight & Cost | DevOps/Architect, `cost-estimator` + `environment-preflight` | token estimate + `docs/project/PREFLIGHT.md` | **HARD STOP** if preflight check fails |
| 1 Planning | Planner, `prd-generator` + `security-context-manager` | `docs/project/PRD.md` + initial `docs/project/THREAT_MODEL.md` | **GATE 1:** user reviews PRD |
| 2 Stack decision | Architect, `stack-advisor` | `STACK.md` + ADR | **GATE 2:** user approves stack |
| 3 Design system | Architect, `design-system-integrator` + `design-system-generator` + `security-context-manager` | `DESIGN.md` + token files + finalized `THREAT_MODEL.md` | **GATE 3:** user reviews tokens & threat model |
| 4 Technical spec | Architect, `tech-spec-writer` | `ARCHITECTURE.md` + `docs/specs/` | **GATE 4:** user reviews specs |
| Pre-5 Analysis | QA/Architect, `artifact-analyzer` | `docs/project/ANALYSIS.md` | **BLOCKS** Phase 5 on `CRITICAL` issue |
| 5 Task breakdown | Planner, `task-breakdown` | `docs/tasks/TASKS.md` (vertical slices + `files_allowed`) | none |
| Pre-6 Snapshot | DevOps/Implementer, `environment-preflight` + `snapshot-rollback` | pre-Phase 6 `PREFLIGHT.md` + snapshot commit hash | **HARD STOP** on preflight fail |
| 6 Implementation | Frontend/Backend, `ui-interaction-authority` (before UI code) + `ponytail-wrapper` + `design-system-integrator` + `boundary-guard` + `context-grounding` + `human-in-the-loop` + `evidence-gated-blocking` | code, `INTERACTION_AUDIT.md`, ticked tasks, `docs/tasks/logs/phase-6.log` | `ui-interaction-authority`, `boundary-guard`, `human-in-the-loop`, & passing log |
| 7 Audit | QA, `viewport-qa` + `cognitive-load-auditor` (mandatory before `design-audit` passes) + `design-audit` + `copy-audit` + `code-quality-gate` + `security-hardening` + `sast-scanner` + `sca-secret-scanner` + `dast-scanner` + `agentic-security-guard` + `security-gate` + `architecture-guard` + `test-suite-auditor` + `meta-judge` + `human-in-the-loop` + `lessons-learned` + `evidence-gated-blocking` + `prompt-history` | Two-Axis audit report (`AUDIT.md` with `## Cognitive Load`) + `docs/tasks/logs/{phase-7,viewport-qa,sast-*,sca-*,dast-*}.log` + `docs/history/PHR-*.md` | **BLOCKED** unless `viewport-qa`, `cognitive-load-auditor`, Axis 1, Axis 2, `meta-judge`, & logs pass (fail returns to Phase 6) |
| Pre-8 Security Gate | Security/QA, `security-gate` | Final `## Security` verdict in `docs/tasks/AUDIT.md` | **HARD BLOCK** on any `HIGH` / `CRITICAL` finding or leaked secret |
| Pre-8 Interaction Gate | QA/Frontend, `viewport-qa` + `cognitive-load-auditor` | Verified `docs/tasks/logs/viewport-qa.log` + `## Cognitive Load` in `docs/tasks/AUDIT.md` | **HARD BLOCK** on any `viewport-qa` or `cognitive-load-auditor` failure |
| 8 Delivery | DevOps, build + CI gate + deployment validation + `prompt-history` | deployment report + `docs/history/PHR-*.md` | user review |

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

## Mandatory V4.1 Integration Rules
- **Between Phase 4 & 5 (`artifact-analyzer`):** Run read-only semantic analysis across `PRD.md`, `ARCHITECTURE.md`, `docs/specs/*.md`, and `00-constitution.md` -> `docs/project/ANALYSIS.md`. Block Phase 5 if any `CRITICAL` issue is found.
- **Phase 6 (`code-quality-gate` 800 LOC Limit):** Stop and split code or task scope whenever a single task or PR exceeds 800 lines of code.
- **Phase 7 (`architecture-guard` & Two-Axis Review):** Verify actual repository structure and imports against `ARCHITECTURE.md` to prevent Architectural Drift. Split Phase 7 audit output into **Axis 1: Agent Fix List** and **Axis 2: Human Decision List** (`"Agents will fix Axis 1. The human must review Axis 2."`).
- **Phase & Workflow Completion (`prompt-history`):** Run `prompt-history` at phase completions and the end of every workflow to append an immutable log to `docs/history/PHR-[YYYYMMDD-HHMM].md`.

## Mandatory V5 Security Fortress Rules
- **Phase 1 & Phase 3 (`security-context-manager`):** Instantiate `.agents/security-context.yaml` into `docs/project/THREAT_MODEL.md` (with `aegis_assess`/`aegis_review` when Aegis MCP is configured), defining trust boundaries, data classifications, authentication, input validation, output encoding, secrets management, and compliance constraints before spec and code generation.
- **Phase 6 & Phase 7 (`sast-scanner` & `sca-secret-scanner`):** Enforce `.agents/skills/security-hardening/references/secure-coding-rules.md` during Phase 6 implementation, run `sast-scanner` (`docs/tasks/logs/sast-[timestamp].log`) and `sca-secret-scanner` (`docs/tasks/logs/sca-[timestamp].log`) on changed files during Phase 6 and across the full workspace in Phase 7, and block on `HIGH`/`CRITICAL` findings, leaked secrets, or `CRITICAL` CVEs.
- **Phase 7 (`dast-scanner` & `agentic-security-guard`):** Run `dast-scanner` (`docs/tasks/logs/dast-[timestamp].log`) via VibeShift/BreachKit MCP when a live runtime endpoint exists (or record explicit static-build skip justification in `docs/tasks/AUDIT.md`), and run `agentic-security-guard` (OWASP Top 10 for Agentic Applications 2026) to write `## Agentic Security` in `docs/tasks/AUDIT.md`.
- **Pre-8 Security Gate (`security-gate`):** Before Phase 8 Delivery, run `security-gate` to synthesize `THREAT_MODEL.md`, SAST, SCA/secret, DAST, and agentic security outputs, invoke `security-mcp` auto-fix + re-scan when available, write `## Security` in `docs/tasks/AUDIT.md`, and issue a binary **BLOCK** on any remaining `HIGH` or `CRITICAL` vulnerability.

## Mandatory V6 Interaction & Cognitive Gate Rules
- **Phase 6 (`ui-interaction-authority`):** Mandatory before generating any form, input, dropdown, date picker, combobox, popover, modal, or toast. Enforce the single designated headless UI library from `STACK.md`, ban unapproved native `<select>`, `<input type="date">`, `<input type="time">`, `alert()`, `confirm()`, and `prompt()`, require `portal={true}` and collision detection (`flip`, `shift`, `autoUpdate`), and record compliance in `docs/project/INTERACTION_AUDIT.md`.
- **Phase 7 (`viewport-qa` & `cognitive-load-auditor`):** Both MUST run and pass **before `design-audit` passes**. Validate all 8 widths (`320` to `1440px`), zero horizontal scroll, zero shell `overflow: hidden` masking, popover portal escape, auth height (`docs/tasks/logs/viewport-qa.log`), plus the Rule of Five, 7-field split, redundancy check, and single primary CTA (`## Cognitive Load` in `docs/tasks/AUDIT.md`).
- **Pre-8 Interaction Gate:** Phase 8 (Delivery) is **BLOCKED** if either `viewport-qa` or `cognitive-load-auditor` fails. Return to Phase 6, apply fixes, and re-verify until both pass.

## Converge step
After Phase 6, and before the Phase 7 audit, check consistency: every `REQ-XXX` is covered by a spec and a task, every task is done or consciously deferred, and code matches `STACK.md` and `DESIGN.md`. Differences are fixed in the spec first, then in code. This is the optional Analyze check from OpenSpec and the Converge step from Spec Kit.

## Handover rules
- Phases hand over through markdown files, not conversation.
- Gates are never skipped. If the user asks to skip, record an accepted risk in `docs/project/DECISIONS.md`.
- Rule precedence is defined in `.agents/rules/03-precedence.md`.
- The stack and design system are decided per project. Never assume them.


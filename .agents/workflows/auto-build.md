# Workflow: /auto-build

Trigger: `/auto-build <idea>` or `/auto-build --review=prd,stack <idea>`

Autonomous end-to-end build pipeline from raw idea to audited delivery.

> **Disclaimer:** `/auto-build` consumes heavy tokens and requires user trust. By default it makes architectural, stack, dependency, and design decisions autonomously without stopping for manual confirmation. Use `/full-cycle` if you want to review every gate and control token usage.

## Flag Parsing
- Extract the application `<idea>` and any `--review=<gates>` flag.
- Supported gate names in `--review`: `prd`, `stack`, `design`, `spec` (comma-separated, for example `--review=prd,stack`).
- If `--review` is omitted, no gates pause for user input.

## Pipeline Process
Execute the full pipeline sequentially: `PRD -> Stack -> Design -> Spec -> Tasks -> Implementation -> Audit -> Delivery`.

1. **Phase 1 -- Planning (PRD):** Planner runs `prd-generator` -> `docs/project/PRD.md`.
   - If `prd` is in `--review`, **PAUSE** and request user approval before continuing.
   - Otherwise, resolve minor ambiguities using sensible defaults, record assumptions in `docs/project/PRD.md` and `docs/project/DECISIONS.md`, and proceed.
2. **Phase 2 -- Stack Decision:** Architect runs `stack-advisor` -> `docs/project/STACK.md` + ADR in `docs/project/DECISIONS.md`.
   - If `stack` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select the best-fit option tied to `REQ-XXX` and NFRs, record the auto-approved stack and dependencies in `DECISIONS.md`, and proceed.
3. **Phase 3 -- Design System:** Architect runs `design-system-applier` (with `design-system-generator` for token export) -> `docs/project/DESIGN.md` + token files / theme provider config.
   - If `design` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select/extract the design system, verify hard-floor contrast and states, record in `DECISIONS.md`, and proceed.
4. **Phase 4 -- Technical Spec:** Architect runs `tech-spec-writer` -> `docs/project/ARCHITECTURE.md` + `docs/specs/*.md` (each under 250 lines).
   - If `spec` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, proceed automatically.
5. **Phase 5 -- Task Breakdown:** Planner runs `task-breakdown` -> `docs/tasks/TASKS.md`.
6. **Phase 6 -- Implementation:** Frontend/Backend implement tasks sequentially using `ponytail-wrapper` and `design-system-applier`.
7. **Phase 7 -- Audit & Autonomous Fix Loop:**
   - Run `design-audit` (hard floor, soft preferences, and visual regression testing), `copy-audit`, `code-quality-gate`, and the test suite.
   - **CRITICAL:** If any test, visual check, design audit, copy audit, or code quality gate fails, do NOT stop and hand over broken code. Autonomously return to Phase 6, fix the discrepancies or defects, and re-run all failing checks until every gate passes.
8. **Phase 8 -- Delivery & Final Report:**
   - DevOps runs the production build, validates `.github/workflows/pr-quality.yaml`, and writes `docs/tasks/DEPLOY.md`.
   - Produce a concise final report summarizing: created files, key autonomous decisions recorded in `DECISIONS.md`, test/audit pass status, and any environment prerequisites or caveats.

## Handover and Audit Rules
- Handover between phases still happens through markdown files (`PRD.md`, `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, `docs/specs/`, `TASKS.md`), preserving a committable trail.
- Every gate bypassed automatically MUST be logged in `docs/project/DECISIONS.md` as an accepted autonomous decision under `/auto-build`.
- Hard-floor design rules, security validation, and `code-quality-gate` checks are never bypassed.


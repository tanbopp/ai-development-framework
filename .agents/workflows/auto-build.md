# Workflow: /auto-build

Trigger: `/auto-build <idea>` or `/auto-build --review=prd,stack <idea>`

Autonomous end-to-end build pipeline from raw idea to audited delivery.

> **Disclaimer:** `/auto-build` consumes heavy tokens and requires user trust. By default it makes architectural, stack, dependency, and design decisions autonomously without stopping for manual confirmation. Use `/full-cycle` if you want to review every gate and control token usage.

## Flag Parsing
- Extract the application `<idea>` and any `--review=<gates>` flag.
- Supported gate names in `--review`: `prd`, `stack`, `design`, `spec` (comma-separated, for example `--review=prd,stack`).
- If `--review` is omitted, no gates pause for user input.

## Pipeline Process
Execute the full pipeline sequentially: `Cost & Preflight -> PRD -> Stack -> Design -> Spec -> Tasks -> Snapshot -> Implementation -> Audit -> Delivery`.

0. **Step 0 -- Cost Estimator & Environment Preflight:** Run `cost-estimator` at the very beginning to evaluate complexity and warn on large projects (offering `/full-cycle` as a step-by-step option). Then run `environment-preflight` -> `docs/project/PREFLIGHT.md`.
   - Check disk space, RAM, baseline tools, and port availability. If any check fails, abort immediately and instruct the user how to fix it.
1. **Phase 1 -- Planning (PRD):** Planner runs `prd-generator` -> `docs/project/PRD.md`.
   - If `prd` is in `--review`, **PAUSE** and request user approval before continuing.
   - Otherwise, resolve minor ambiguities using sensible defaults, record assumptions in `docs/project/PRD.md` and `docs/project/DECISIONS.md`, and proceed.
2. **Phase 2 -- Stack Decision:** Architect runs `stack-advisor` -> `docs/project/STACK.md` + ADR in `docs/project/DECISIONS.md`.
   - If `stack` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select the best-fit option tied to `REQ-XXX` and NFRs, record the auto-approved stack and dependencies in `DECISIONS.md`, and proceed.
3. **Phase 3 -- Design System:** Architect runs `design-system-integrator` (with `design-system-generator` for token export) -> `docs/project/DESIGN.md` + token files / theme provider config.
   - If `design` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select/extract the design system, verify hard-floor contrast and states, record in `DECISIONS.md`, and proceed.
4. **Phase 4 -- Technical Spec:** Architect runs `tech-spec-writer` -> `docs/project/ARCHITECTURE.md` + `docs/specs/*.md` (each under 250 lines).
   - If `spec` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, proceed automatically.
5. **Phase 5 -- Task Breakdown:** Planner runs `task-breakdown` -> `docs/tasks/TASKS.md` (with `files_allowed` per task).
   - **Pre-Phase 6 Safety Gate:** Re-run `environment-preflight` against `STACK.md` requirements (including 20 GB disk check for Desktop/Mobile and toolchains in `PATH`), then run `snapshot-rollback` to record a clean Git recovery hash in `docs/project/PREFLIGHT.md`.
6. **Phase 6 -- Implementation:** Frontend/Backend implement tasks sequentially using `ponytail-wrapper` and `design-system-integrator`.
   - **Mandatory `boundary-guard`:** Check `files_allowed` in `TASKS.md` before every file edit; halt if a path is outside scope or `files_allowed` is missing.
   - **Mandatory `context-grounding`:** Before writing imports or using APIs, run `context-grounding`.
   - **Mandatory `human-in-the-loop`:** Pause for explicit user approval before modifying critical files (auth, migrations, payments, `.env`) or proceeding on low-confidence changes.
   - **Mandatory `evidence-gated-blocking`:** Capture raw build/test/typecheck terminal output in `docs/tasks/logs/phase-6.log` with `exit code 0` before closing Phase 6.
   - Run `snapshot-rollback` after completing major features.
7. **Phase 7 -- Audit & Autonomous Fix Loop:**
   - Run `design-audit` (hard floor, soft preferences, and visual regression testing), `copy-audit`, `code-quality-gate`, `security-hardening`, `test-suite-auditor`, and the test suite, enforcing `human-in-the-loop` if any fix touches critical files or has low confidence.
   - Run `meta-judge` after `design-audit` and `security-hardening` to cross-examine audit results and enforce zero corner-cutting.
   - **Mandatory `lessons-learned`:** If any test or audit fails, run `lessons-learned` to record the failure.
   - **Mandatory `evidence-gated-blocking` Gate:** Save raw terminal audit/test verification output to `docs/tasks/logs/phase-7.log`. Phase 7 is BLOCKED and Phase 8 delivery is strictly forbidden unless the log exists with zero errors and `exit code 0`.
   - **CRITICAL:** If any test, visual check, design audit, copy audit, code quality gate, `test-suite-auditor`, `meta-judge`, or `evidence-gated-blocking` check fails, do NOT stop and hand over broken code. Autonomously return to Phase 6, fix the discrepancies or defects, and re-run all failing checks until every gate passes.
8. **Phase 8 -- Delivery & Final Report:**
   - DevOps runs the production build, validates `.github/workflows/pr-quality.yaml`, and writes `docs/tasks/DEPLOY.md`.
   - Produce a concise final report summarizing: created files, key autonomous decisions recorded in `DECISIONS.md`, test/audit pass status, and any environment prerequisites or caveats.

## Handover and Audit Rules
- Handover between phases still happens through markdown files (`PREFLIGHT.md`, `PRD.md`, `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, `docs/specs/`, `TASKS.md`), preserving a committable trail.
- Every gate bypassed automatically MUST be logged in `docs/project/DECISIONS.md` as an accepted autonomous decision under `/auto-build`.
- Hard-floor design rules, security validation, `environment-preflight`, `boundary-guard`, `context-grounding`, `human-in-the-loop`, `lessons-learned`, `test-suite-auditor`, `meta-judge`, `evidence-gated-blocking`, and `code-quality-gate` checks are never bypassed.


# Workflow: /auto-build

Trigger: `/auto-build <idea>` or `/auto-build --review=prd,stack <idea>`

Autonomous end-to-end build pipeline from raw idea to audited delivery.

> **Disclaimer:** `/auto-build` consumes heavy tokens and requires user trust. By default it makes architectural, stack, dependency, and design decisions autonomously without stopping for manual confirmation. Use `/full-cycle` if you want to review every gate and control token usage.

## Flag Parsing
- Extract the application `<idea>` and any `--review=<gates>` flag.
- Supported gate names in `--review`: `prd`, `stack`, `design`, `spec` (comma-separated, for example `--review=prd,stack`).
- If `--review` is omitted, no gates pause for user input.

## Pipeline Process
Execute the full pipeline sequentially: `Cost & Preflight -> PRD -> Stack -> Design -> Spec -> Artifact Analysis -> Tasks -> Snapshot -> Implementation -> Audit (Two-Axis) -> Security Gate -> Delivery`.

0. **Step 0 -- Cost Estimator & Environment Preflight:** Run `cost-estimator` at the very beginning to evaluate complexity and warn on large projects (offering `/full-cycle` as a step-by-step option). Then run `environment-preflight` -> `docs/project/PREFLIGHT.md`.
   - Check disk space, RAM, baseline tools, and port availability. If any check fails, abort immediately and instruct the user how to fix it.
1. **Phase 1 -- Planning (PRD):** Planner runs `prd-generator` + `security-context-manager` -> `docs/project/PRD.md` + initial `docs/project/THREAT_MODEL.md` (from `.agents/security-context.yaml`).
   - If `prd` is in `--review`, **PAUSE** and request user approval before continuing.
   - Otherwise, resolve minor ambiguities using sensible defaults, record assumptions in `docs/project/PRD.md` and `docs/project/DECISIONS.md`, and proceed.
2. **Phase 2 -- Stack Decision:** Architect runs `stack-advisor` -> `docs/project/STACK.md` + ADR in `docs/project/DECISIONS.md`.
   - If `stack` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select the best-fit option tied to `REQ-XXX` and NFRs, record the auto-approved stack and dependencies in `DECISIONS.md`, and proceed.
3. **Phase 3 -- Design System:** Architect runs `design-system-integrator` (with `design-system-generator` for token export) and `security-context-manager` -> `docs/project/DESIGN.md`, token files / theme provider config, and finalized `docs/project/THREAT_MODEL.md`.
   - If `design` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, select/extract the design system, verify hard-floor contrast and states, record in `DECISIONS.md`, and proceed.
4. **Phase 4 -- Technical Spec:** Architect runs `tech-spec-writer` -> `docs/project/ARCHITECTURE.md` + `docs/specs/*.md` (each under 250 lines).
   - If `spec` is in `--review`, **PAUSE** and request user approval.
   - Otherwise, proceed automatically.
   - **Pre-Phase 5 Consistency Gate (`artifact-analyzer`):** Run read-only cross-artifact analysis across `PRD.md`, `ARCHITECTURE.md`, `docs/specs/*.md`, and `00-constitution.md` -> `docs/project/ANALYSIS.md`. If any `CRITICAL` issue is found, block Phase 5 and resolve the spec contradictions before generating tasks.
5. **Phase 5 -- Task Breakdown:** Planner runs `task-breakdown` -> `docs/tasks/TASKS.md` (with `files_allowed` per task).
   - **Pre-Phase 6 Safety Gate:** Re-run `environment-preflight` against `STACK.md` requirements (including 20 GB disk check for Desktop/Mobile and toolchains in `PATH`), then run `snapshot-rollback` to record a clean Git recovery hash in `docs/project/PREFLIGHT.md`.
6. **Phase 6 -- Implementation:** Frontend/Backend implement tasks sequentially using `ponytail-wrapper`, `design-system-integrator`, `docs/project/THREAT_MODEL.md`, and `.agents/skills/security-hardening/references/secure-coding-rules.md`.
   - **Mandatory `boundary-guard`:** Check `files_allowed` in `TASKS.md` before every file edit; halt if a path is outside scope or `files_allowed` is missing.
   - **Mandatory `context-grounding`:** Before writing imports or using APIs, run `context-grounding`.
   - **Mandatory `human-in-the-loop`:** Pause for explicit user approval before modifying critical files (auth, migrations, payments, `.env`) or proceeding on low-confidence changes.
   - **Mandatory `code-quality-gate` (800 LOC Limit):** Stop and split code or task scope if a single task/PR exceeds 800 lines of code.
   - **Mandatory V5 Continuous Checks:** Run `sast-scanner` and `sca-secret-scanner` on changed files/dependencies before closing implementation tasks.
   - **Mandatory `evidence-gated-blocking`:** Capture raw build/test/typecheck terminal output in `docs/tasks/logs/phase-6.log` with `exit code 0` before closing Phase 6.
   - Run `snapshot-rollback` after completing major features.
7. **Phase 7 -- Audit & Autonomous Fix Loop (Two-Axis Review):**
   - Run `design-audit` (hard floor, soft preferences, and visual regression testing), `copy-audit`, `code-quality-gate`, `security-hardening`, `sast-scanner`, `sca-secret-scanner`, `dast-scanner` (when the built app runs locally; otherwise record explicit static-build skip justification in `docs/tasks/AUDIT.md`), `agentic-security-guard` (writing `## Agentic Security` in `docs/tasks/AUDIT.md`), `architecture-guard` (Architectural Drift check against `ARCHITECTURE.md`), `test-suite-auditor`, and the test suite, enforcing `human-in-the-loop` if any fix touches critical files or has low confidence.
   - Run `meta-judge` after `design-audit` and `security-hardening` to cross-examine audit results, enforce zero corner-cutting, and split output into a **Two-Axis Review**:
     - **Axis 1: Agent Fix List** (technical findings fixed autonomously in Phase 6).
     - **Axis 2: Human Decision List** (product/intent questions surfaced explicitly for user decision).
     - Explicit audit footer: `"Agents will fix Axis 1. The human must review Axis 2."`
   - **Mandatory `lessons-learned`:** If any test or audit fails, run `lessons-learned` to record the failure.
   - **Mandatory `evidence-gated-blocking` Gate:** Save raw terminal audit/test verification output to `docs/tasks/logs/phase-7.log` alongside `docs/tasks/logs/sast-[timestamp].log`, `docs/tasks/logs/sca-[timestamp].log`, and (when applicable) `docs/tasks/logs/dast-[timestamp].log`. Phase 7 is BLOCKED and Phase 8 delivery is strictly forbidden unless the logs exist with zero errors and `exit code 0`.
   - **CRITICAL:** If any Axis 1 item, test, visual check, design audit, copy audit, code quality gate, `sast-scanner`, `sca-secret-scanner`, `dast-scanner`, `agentic-security-guard`, `architecture-guard`, `test-suite-auditor`, `meta-judge`, or `evidence-gated-blocking` check fails, do NOT stop and hand over broken code. Autonomously return to Phase 6, fix the discrepancies or defects, and re-run all failing checks until every Axis 1 gate passes.
   - **Pre-8 Security Gate (`security-gate`):** Immediately before Phase 8, run `security-gate` to synthesize `THREAT_MODEL.md`, SAST, SCA/secret, DAST, and agentic security reports. Invoke `security-mcp` autonomous fixes + re-scan when available (respecting `human-in-the-loop` on critical files), write `## Security` in `docs/tasks/AUDIT.md`, and **BLOCK** Phase 8 delivery on any remaining `HIGH` or `CRITICAL` finding.
8. **Phase 8 -- Delivery & Final Report:**
   - DevOps runs the production build, validates `.github/workflows/pr-quality.yaml`, writes `docs/tasks/DEPLOY.md`, and runs `prompt-history` to log the completed workflow in `docs/history/PHR-[YYYYMMDD-HHMM].md`.
   - Produce a concise final report summarizing: created files, key autonomous decisions recorded in `DECISIONS.md`, any open Axis 2 Human Decision items, test/audit pass status, PHR path, and environment caveats.

## Handover and Audit Rules
- Handover between phases still happens through markdown files (`PREFLIGHT.md`, `PRD.md`, `THREAT_MODEL.md`, `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, `ANALYSIS.md`, `docs/specs/`, `TASKS.md`, `AUDIT.md`, `docs/history/PHR-*.md`), preserving an immutable committable trail.
- Every gate bypassed automatically MUST be logged in `docs/project/DECISIONS.md` as an accepted autonomous decision under `/auto-build`.
- Hard-floor design rules, security validation, `environment-preflight`, `boundary-guard`, `context-grounding`, `human-in-the-loop`, `lessons-learned`, `artifact-analyzer`, `architecture-guard`, `test-suite-auditor`, `meta-judge`, `evidence-gated-blocking`, `prompt-history`, `code-quality-gate` (including the 800 LOC Limit), `security-context-manager`, `sast-scanner`, `sca-secret-scanner`, `dast-scanner`, `agentic-security-guard`, and `security-gate` are never bypassed.


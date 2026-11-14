---
name: meta-judge
description: Cross-examines Phase 7 audit findings after design-audit and security-hardening to detect corner-cutting, bypassed rules, or relaxed thresholds before delivery is allowed.
version: 4.0.0
status: active
---

# Meta-Judge

## Trigger
Mandatory during Phase 7 (Audit), immediately after `design-audit` and `security-hardening` (plus `copy-audit`, `code-quality-gate`, `architecture-guard`, and `test-suite-auditor`) complete.

## Process
1. **Cross-Examine All Audit Artifacts:**
   - Review every Phase 7 audit result alongside `docs/project/PRD.md`, `docs/project/STACK.md`, `docs/project/DESIGN.md`, `docs/project/ARCHITECTURE.md`, and `docs/tasks/TASKS.md`.
2. **Detect Soft Passes & Rule Bypassing:**
   - Inspect whether any check "went soft" or waived hard requirements just to finish faster (for example: ignoring WCAG AA contrast failures, omitting focus states or `prefers-reduced-motion`, skipping Zod/input validation, permitting undocumented `any` escape hatches, or using unjustified Design Overrides).
3. **Conduct Mandatory Self-Interrogation:**
   - Evaluate explicitly against repository standards: *"Did I actually follow `.agents/SOUL.md` and `.agents/rules/`? Or did I cut corners?"*
   - Verify whether any `human-in-the-loop`, `boundary-guard`, or `context-grounding` requirement was sidestepped during implementation or fixes.
4. **Enforce Remediation Before Delivery:**
   - If any corner-cutting, missing evidence, or relaxed threshold is detected, **FAIL** the Meta-Judge review, record the root cause via `lessons-learned`, and block Phase 8 delivery until fixes are implemented in Phase 6 and re-audited.
5. **Two-Axis Review Split (Mandatory Output Structure):**
   At the end of the audit, split all findings into two distinct sections:
   - **Axis 1: Agent Fix List:** Technical findings that the AI MUST fix automatically (for example, WCAG contrast < 4.5, missing `alt` tags, TypeScript `any`, Architectural Drift, or failed unit tests).
   - **Axis 2: Human Decision List:** Findings that require human intent or judgment (for example, "Is this UX flow intuitive?", "Should this API be public?", or "Trade-off between speed and battery").

## Output Contract
A written Two-Axis audit verdict (`Axis 1: Agent Fix List` and `Axis 2: Human Decision List`) confirming strict compliance across `.agents/SOUL.md`, `.agents/rules/`, `PRD.md`, `DESIGN.md`, and all prior Phase 7 audits. The audit report MUST explicitly state: **"Agents will fix Axis 1. The human must review Axis 2."**

## Checklist
- [ ] All prior Phase 7 audit reports cross-examined against specs and hard rules
- [ ] Zero waived hard-floor design rules or omitted security/input-validation checks
- [ ] Explicit self-check performed against `.agents/SOUL.md` and `.agents/rules/`
- [ ] Output split cleanly into **Axis 1: Agent Fix List** and **Axis 2: Human Decision List**
- [ ] Any detected corner-cutting or Axis 1 item blocks Phase 8 and triggers remediation plus `lessons-learned`

## Prohibitions
- Rubber-stamping prior audit passes without inspecting underlying code, tokens, and logs.
- Approving Phase 8 delivery when hard rules, `SOUL.md` standards, or `REQ-XXX` criteria were compromised.

---
name: artifact-analyzer
description: Performs a read-only semantic cross-artifact analysis across PRD.md, ARCHITECTURE.md, and docs/specs/*.md between Phase 4 and Phase 5 to block ambiguity, inconsistency, duplication, and constitution violations.
version: 4.1.0
status: active
---

# Artifact Consistency Analyzer

## Trigger
Phase 4 (between Technical Spec and Task Breakdown), before invoking `task-breakdown`.

## Process
1. Read `docs/project/PRD.md`, `docs/project/STACK.md`, `docs/project/DESIGN.md`, `docs/project/ARCHITECTURE.md`, `docs/specs/*.md`, and `.agents/rules/00-constitution.md`.
2. Perform a strictly read-only semantic analysis across all artifacts to detect:
   - **Ambiguity:** Requirements or EARS acceptance criteria that are unclear, untestable, or lack binary pass/fail outcomes.
   - **Inconsistency:** Contradictions across documents (for example, `PRD.md` or `STACK.md` states PostgreSQL, while `ARCHITECTURE.md` or a feature spec states MongoDB).
   - **Duplication:** Overlapping `REQ-XXX` entries or redundant/conflicting behavior definitions across specs.
   - **Constitution Violations:** Any spec breaching `.agents/rules/00-constitution.md` (missing `REQ-XXX` IDs, missing binary AC, missing out-of-scope, unapproved dependency, spec over 250 lines).
3. Classify findings by severity (`CRITICAL` or `WARNING`).
4. **Gate Enforcement:** If any `CRITICAL` issue is found, **BLOCK** progression to Phase 5 (`task-breakdown`). Output the full list of findings and instruct the agent/Architect to resolve the spec defects before re-running analysis.
5. Save the complete analysis report to `docs/project/ANALYSIS.md`.

## Output Contract
A saved `docs/project/ANALYSIS.md` report classifying cross-artifact findings (`PASS` or `BLOCKED` on any `CRITICAL` finding).

## Checklist
- [ ] `PRD.md`, `ARCHITECTURE.md`, and all `docs/specs/*.md` inspected read-only
- [ ] Ambiguity, Inconsistency, Duplication, and Constitution rules explicitly evaluated
- [ ] Phase 5 blocked if any `CRITICAL` finding exists
- [ ] Report written to `docs/project/ANALYSIS.md`

## Prohibitions
- Silently editing source specs during the read-only analysis pass to hide contradictions.
- Advancing to Phase 5 (`task-breakdown`) while any `CRITICAL` inconsistency or constitution violation remains open.

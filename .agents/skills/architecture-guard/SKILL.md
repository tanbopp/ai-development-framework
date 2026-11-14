---
name: architecture-guard
description: Verifies during Phase 7 audit that actual repository directory structure, dependencies, and module imports conform to docs/project/ARCHITECTURE.md, flagging Architectural Drift and requiring refactoring.
version: 4.1.0
status: active
---

# Architecture Guard

## Trigger
Mandatory during Phase 7 (Audit), prior to `meta-judge` sign-off and Phase 8 delivery.

## Process
1. Read `docs/project/ARCHITECTURE.md` and `docs/specs/*.md` to load the approved directory layout, architectural boundaries, layer rules, and allowed dependency graph.
2. Inspect the actual codebase structure and cross-module imports (`import`, `require`, package references) across implemented files.
3. Detect **Architectural Drift**, including:
   - Layer boundary violations (for example, a UI component importing directly from the database or ORM layer instead of going through approved service/API boundaries).
   - Unapproved folders, orphaned modules, or circular dependencies between bounded contexts not defined in `ARCHITECTURE.md`.
   - Unauthorized third-party dependencies not recorded in `docs/project/STACK.md` or `docs/project/DECISIONS.md`.
4. If any Architectural Drift is detected:
   - Flag each violation with exact `file:line` and the violated architectural rule.
   - Classify code boundary violations under **Axis 1: Agent Fix List** (and any structural spec conflicts under **Axis 2: Human Decision List** if spec intent is unclear), blocking Phase 8 delivery until refactored cleanly.

## Output Contract
An architectural compliance verdict confirming zero Architectural Drift against `docs/project/ARCHITECTURE.md`, or a file-level refactor list blocking Phase 8.

## Checklist
- [ ] Actual directory layout matches `docs/project/ARCHITECTURE.md`
- [ ] Module imports obey layer boundaries (no UI-to-database shortcuts or forbidden cross-layer coupling)
- [ ] Zero unapproved packages or unrequested structural deviations
- [ ] Any Architectural Drift blocks Phase 8 until refactored

## Prohibitions
- Approving Phase 7 when UI or presentation layers bypass API/service boundaries into persistence internals.
- Silently modifying `ARCHITECTURE.md` to match accidental implementation drift without ADR review.

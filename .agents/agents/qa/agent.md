# QA Agent

## Role
Auditor. Runs `design-audit` and `copy-audit`, and verifies test coverage.

## Responsibilities
- Reject output that violates the hard floor.
- For soft preferences, check for a written justification in `DESIGN.md` before rejecting.
- Write a specific, actionable fix list when output fails.

## Output Contract
- Audit report with pass/fail status per item, its tier (hard/soft), and an actionable fix list.

## Prohibitions
- Going soft on the hard floor.
- Generating new design.
- Ignoring the hard floor.

## Handover
- Reads: implemented code, `DESIGN.md`, `docs/specs/*.md`, `TASKS.md`.
- Writes: audit report (`docs/tasks/AUDIT.md`).
- Next: Frontend (on failure, with the fix list) or DevOps (on pass).


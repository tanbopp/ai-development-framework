# Workflow: /plan-feature

Trigger: `/plan-feature <idea>`

## Process
1. Planner runs `prd-generator` on the idea.
2. Output: `docs/project/PRD.md` (or an index plus `docs/specs/PRD-<feature>.md` files).
3. **GATE 1:** stop and request user approval.

Do not proceed to stack selection before the PRD is approved. If the user asks to skip the gate, record an accepted risk in `docs/project/DECISIONS.md`.


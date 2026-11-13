---
name: design-audit
description: Audits frontend output against a hard floor (must fail) and soft preferences (overridable with written justification in DESIGN.md) using Hallmark and unslop as auditors only. Use before delivering any frontend output.
---

# Design Audit

## Trigger
Before delivering frontend output.

## Process
1. Hard-floor checks. Any violation fails the audit: WCAG AA contrast, focus state, `prefers-reduced-motion`, mobile overflow, truncated text.
2. Soft-preference checks (Hallmark Reflex and Finish tiers, unslop rules). On violation, look in `DESIGN.md` for a written brand justification. If present, pass. If absent, request a fix.
3. Produce a per-item report: pass/fail, tier (hard/soft), rationale, suggested fix.

Tools (audit only, never generate): `node .agents/vendor/hallmark/skills/hallmark/scripts/sloplint.mjs <path> --render` or `/hallmark audit <path>`; `npx -y unslop-mcp audit . --json --no-fail`. If a tool is unavailable, perform the checks manually and say so in the report.

## Output Contract
An audit report. Reject on any hard-floor violation. Reject on soft preferences only when no justification exists.

## Checklist
- [ ] All five hard-floor checks run
- [ ] Every soft failure checked against `DESIGN.md`
- [ ] Report lists tier for each item

## Prohibitions
- Imposing a single design taste on every project.
- Going soft on the hard floor.
- Generating new design.


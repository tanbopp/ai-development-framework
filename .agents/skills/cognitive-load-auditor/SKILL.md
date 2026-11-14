---
name: cognitive-load-auditor
description: Audits every page and modal against progressive disclosure rules (Rule of Five, 7-field split, redundancy test, single primary CTA) in Phase 7 and writes Cognitive Load verdicts to docs/tasks/AUDIT.md.
version: 6.0.0
status: active
---

# Cognitive Load Auditor

## Trigger
Phase 7 (Audit, mandatory before `design-audit` passes) and mandatory Pre-Phase 8 Interaction Gate before Phase 8 (Delivery).

## Process
1. **Inventory Every Page and Modal:**
   - Scan every page, route, and modal dialog. Count visible interactive form fields, key metrics, displayed summary values, and primary actions.
2. **Rule of Five:**
   - Verify no single viewport displays more than **5 key metrics or primary form fields** without progressive disclosure (tabs, accordions, steppers, or "Show more" toggles).
3. **Density Test:**
   - If any form has more than **7 fields**, enforce splitting it into steps or sections with clear headings.
4. **Redundancy Test:**
   - Flag any information displayed twice on the same screen without a clear reason (for example, showing total price in both the header and the footer).
5. **Action Clarity:**
   - Verify there is **exactly one primary action** per view, and that all secondary actions are visually subordinate (secondary, outline, ghost, or link styles).
6. **Fail and Propose Progressive-Disclosure Refactor:**
   - If density, redundancy, or action hierarchy fails thresholds, **FAIL** the audit (blocking `design-audit` sign-off and Phase 8 delivery) and propose a concrete progressive-disclosure refactor before returning to Phase 6.

## Output Contract
`docs/tasks/AUDIT.md` updated with a **`## Cognitive Load`** section listing each page/modal, its visible element count (fields, metrics, primary actions), redundancy findings, and the binary `PASS`/`FAIL` verdict.

## Checklist
- [ ] Scanned every page and modal for visible fields, metrics, and actions
- [ ] Enforced Rule of Five (<= 5 key metrics or primary form fields without progressive disclosure)
- [ ] Enforced Density Test (> 7 form fields split into steps or headed sections)
- [ ] Flagged and resolved duplicate/redundant on-screen information
- [ ] Verified exactly one visually dominant primary action per view
- [ ] Written **`## Cognitive Load`** section with per-page counts and verdict to `docs/tasks/AUDIT.md`

## Prohibitions
- Allowing cluttered forms (> 7 ungrouped fields) or metric walls (> 5 metrics without progressive disclosure) to ship.
- Allowing competing primary CTA buttons on the same view or skipping the **`## Cognitive Load`** section in `docs/tasks/AUDIT.md`.

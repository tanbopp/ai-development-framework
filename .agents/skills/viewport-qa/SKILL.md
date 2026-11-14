---
name: viewport-qa
description: Verifies zero horizontal overflow, bans shell overflow:hidden masking, tests popover portal escape across 8 viewports (320px-1440px), checks auth page height, and blocks Phase 8 on failure.
version: 6.0.0
status: active
---

# Viewport QA

## Trigger
Phase 7 (Audit, mandatory before `design-audit` passes) and mandatory Pre-Phase 8 Interaction Gate before Phase 8 (Delivery).

## Process
1. **Run Multi-Viewport Browser Checks:**
   - Load the built page (or running dev server) in a real headless browser across all 8 required widths: **320, 360, 390, 414, 768, 1024, 1280, 1440px**.
   - Run `unslop-preflight` (`node scripts/scan-viewport-fit.mjs <url>` and `node scripts/scan-ui-implementation.mjs src`) and `designlang verify-ui <url> --brand DESIGN.md` when available, or Playwright/Puppeteer DOM checks.
2. **Horizontal Scroll Check:**
   - Assert at every tested viewport width that `document.documentElement.scrollWidth <= window.innerWidth` (zero horizontal overflow).
3. **Blanket Overflow Ban:**
   - Assert that `html`, `body`, `#root`, and top-level page shells do not use `overflow: hidden` to hide layout bugs.
4. **Popover Escape Test:**
   - Open every dropdown, select, date picker, and popover across the UI. Assert that the floating content is fully visible inside the viewport (not clipped by ancestor containers) and mounts as a direct child of `document.body` or a top-layer container.
5. **Auth Page Height:**
   - Verify auth pages fit without visible vertical scroll at normal content size and recover safely without clipping inputs or submit actions when the mobile on-screen keyboard opens.
6. **Block Delivery on Any Failure:**
   - If any viewport width, shell overflow rule, popover escape test, or auth height check fails, **BLOCK Phase 8 (Delivery)** (`FAIL`), return a precise, actionable fix list (component/selector, viewport width, and DOM telemetry), and loop back to Phase 6.

## Output Contract
`docs/tasks/logs/viewport-qa.log` recording pass/fail status per viewport width (`320` to `1440px`), shell `overflow` validation, auth page height check, and pass/fail status per floating element.

## Checklist
- [ ] Tested all 8 viewport widths (`320`, `360`, `390`, `414`, `768`, `1024`, `1280`, `1440px`)
- [ ] Verified `document.documentElement.scrollWidth <= window.innerWidth` at every width
- [ ] Verified `html`, `body`, `#root`, and page shells do not mask layout bugs with `overflow: hidden`
- [ ] Opened every floating element and verified unclipped visibility inside `document.body` or top-layer
- [ ] Verified auth page height and keyboard recovery
- [ ] Results saved to `docs/tasks/logs/viewport-qa.log`

## Prohibitions
- Shipping or passing `design-audit` when horizontal overflow, clipped popovers, or blanket `overflow: hidden` shells exist.
- Testing only desktop widths or passing `viewport-qa` without logging all 8 widths in `docs/tasks/logs/viewport-qa.log`.

---
name: design-audit
description: Audits frontend output against a hard floor (must fail) and soft preferences (overridable with written justification in DESIGN.md) using Hallmark and unslop as auditors only. Use before delivering any frontend output.
version: 4.0.0
status: active
---

# Design Audit

## Trigger
Before delivering frontend output.

## Process
1. **Hard-floor checks.** Any violation fails the audit: WCAG AA contrast, focus state, `prefers-reduced-motion`, mobile overflow, truncated text.
2. **Soft-preference checks** (Hallmark Reflex and Finish tiers, unslop rules). On violation, look in `docs/project/DESIGN.md` for a written brand justification. If present, pass. If absent, request a fix.
3. **Screenshot capture.** Render the built page in a real browser and capture screenshots (using Playwright, Puppeteer, or `sloplint.mjs --render`).
4. **Visual Regression Testing & Token Match.**
   - If the user provided a reference image or URL in Phase 1 (or recorded one in `DESIGN.md`), compare the captured screenshot with the reference using an MCP visual regression tool (`vrt-mcp` or `zerovisual`).
   - Verify that rendered colors, typography, and spacing match the approved tokens in `docs/project/DESIGN.md`.
   - If visual mismatches exceed 5% or token colors do not match `DESIGN.md`, **REJECT** the implementation and send a concrete list of visual discrepancies to the Frontend Agent.
5. **Deterministic Pixel & Token Audit.**
   - If `plumb-cli` or `onsystem` MCP is available, run it to perform a deterministic pixel-perfect check against the tokens defined in `docs/project/DESIGN.md`. If unavailable, fallback to Hallmark/unslop.
6. Produce a per-item report: pass/fail, tier (hard/soft/visual-regression/deterministic), rationale, suggested fix.

Tools (audit only, never generate):
- Deterministic audit: `plumb-cli`, `onsystem`, or `@usespectra/mcp` MCP tools when available.
- Hallmark: `node <sloplint-path> <path> --render` or `/hallmark audit <path>`. Hallmark's sloplint script may be at either path depending on how Hallmark was installed:
  - `.agents/skills/hallmark/scripts/sloplint.mjs` (installed via `npx skills add`)
  - `.agents/vendor/hallmark/skills/hallmark/scripts/sloplint.mjs` (cloned manually)

  Use whichever exists. If neither exists, Hallmark is not installed -- report this and stop the audit.
- unslop: `npx -y unslop-mcp audit . --json --no-fail`. If unavailable, perform the checks manually and say so in the report.
- Visual regression: `vrt-mcp` or `zerovisual` MCP tools, paired with Playwright or Puppeteer screenshots.

## Output Contract
An audit report. Reject on any hard-floor violation, any deterministic token mismatch, any visual regression diff above 5%, or any token mismatch against `DESIGN.md`. Reject on soft preferences only when no justification exists.

## Checklist
- [ ] All five hard-floor checks run
- [ ] Every soft failure checked against `DESIGN.md`
- [ ] Deterministic token check run via `plumb-cli` / `onsystem` MCP (or fallback to Hallmark/unslop recorded)
- [ ] Screenshot captured and rendered tokens verified against `DESIGN.md`
- [ ] Visual regression check run when a reference exists (diff <= 5%)
- [ ] Report lists tier for each item

## Prohibitions
- Imposing a single design taste on every project.
- Going soft on the hard floor.
- Generating new design.


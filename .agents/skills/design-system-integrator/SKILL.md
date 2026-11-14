---
name: design-system-integrator
description: Integrates design systems via a strict 4-scenario Decision Tree (Figma, existing codebase, brand/URL reference, or zero-design generation) using MCP tools and vendor libraries, and enforces Phase 6 component reuse. Use in Phase 3 and Phase 6.
version: 4.0.0
status: active
---

# Design System Integrator

## Trigger
Phase 3 (Design System) and Phase 6 (Implementation).

## Process

### Phase 3: Strict Decision Tree
Read `docs/project/PRD.md`, `docs/project/STACK.md`, `.agents/rules/02-design-principles.md`, and read `.agents/skills/design-system-integrator/references/mcp-design-tools.md` on-demand for detailed MCP tool workflows. Select the matching scenario:

- **Scenario A (User has Figma):** Use `figma-console-mcp` or `design-context-bridge` to extract DTCG tokens, typography, and component specs, and map them to `docs/project/DESIGN.md`.
- **Scenario B (User has an existing codebase):** Scan CSS/Tailwind config to mine the *de-facto* styling and formalize it into `docs/project/DESIGN.md`.
- **Scenario C (User has a brand/URL reference):** Use `@designmdcc/mcp` or `designlang` to extract the design language from the URL and record it in `docs/project/DESIGN.md`.
- **Scenario D (User has NO design):** Use `standby-design-mcp` or `.agents/vendor/design-taste-frontend` (with `design-system-generator`) to generate a production-ready system from PRD tone and industry.

Ensure `docs/project/DESIGN.md` meets all hard-floor rules (WCAG AA 4.5:1 contrast, focus states, `prefers-reduced-motion`, mobile fit, >= 3 typography levels) and configure the stack's token files / theme provider.

### Phase 6: Component & Token Reuse Rule
Before generating UI components in Phase 6, you MUST use `ds-pilot` or `brandrail` MCP (if available; see `references/mcp-design-tools.md`) to search the local codebase for existing components and tokens to prevent duplication. Bind every component to `docs/project/DESIGN.md` tokens.

## Output Contract
- `docs/project/DESIGN.md` recording the Decision Tree scenario (A-D), extracted or generated tokens, hard-floor compliance, and override justifications.
- Configured theme provider and exported token files matching `docs/project/STACK.md`.

## Checklist
- [ ] Decision Tree scenario (A: Figma, B: Existing codebase, C: Brand/URL, D: No design) applied cleanly
- [ ] Read `.agents/skills/design-system-integrator/references/mcp-design-tools.md` for MCP extraction/reuse details
- [ ] Hard-floor rules satisfied and documented in `docs/project/DESIGN.md`
- [ ] Phase 6 UI component generation preceded by `ds-pilot` / `brandrail` reuse search

## Prohibitions
- Defaulting to `shadcn/ui` or hardcoded styles when another system, Figma spec, codebase, or brand aesthetic applies.
- Guessing tokens from memory instead of invoking MCP/codebase tools.
- Creating duplicate UI components without searching existing components and tokens first.

---
name: design-system-applier
description: Discovers, installs, and applies any requested UI design system (MUI, Ant Design, Bootstrap, Chakra, Tailwind, custom) or extracts tokens from brand/URL references via MCP tools before UI implementation. Use in Phase 3 (Design System) and Phase 6 (Implementation).
---

# Design System Applier

## Trigger
Phase 3 (Design System) and Phase 6 (Implementation).

## Process
1. Read `docs/project/PRD.md` and `docs/project/STACK.md` to identify the requested design system, component library, or visual aesthetic.
2. **If the user specified a known UI system** (for example MUI, Ant Design, Bootstrap, Chakra UI, Tailwind):
   - Confirm dependency approval (per `.agents/rules/00-constitution.md`, or record the autonomous selection in `docs/project/DECISIONS.md` under `/auto-build`).
   - Install the package using the stack's standard CLI (for example `npm install`).
   - Locate or create the framework's theme provider and configuration entry points so all components inherit centralized tokens.
3. **If the user specified a brand aesthetic or reference URL** (for example "like Stripe", "like Linear", "Apple-like", or a live URL):
   - Use available MCP token-discovery tools (such as `better-design` or `designlang`) to fetch colors, typography scales, spacing, radii, and elevation tokens from the reference.
   - If the reference is a user-supplied URL or image, record its location in `docs/project/DESIGN.md` so Phase 7 visual regression can compare against it.
4. **If no design system or brand reference is specified**:
   - Use `inspo` MCP to find an industry-appropriate visual reference based on the PRD's domain and tone.
   - If `inspo` MCP is unavailable, select ONE matching library from `.agents/vendor/design-taste-frontend/` (or derive custom tokens via `design-system-generator`) and document the choice in `docs/project/DESIGN.md`.
5. Generate or update `docs/project/DESIGN.md` mapping every discovered token (colors, typography hierarchy, spacing, radii, shadows, motion, and component states) to the chosen stack, preserving the hard-floor rules in `.agents/rules/02-design-principles.md`.
6. During Phase 6 (Implementation), bind components strictly to the installed theme provider and `DESIGN.md` tokens; do not fall back to `shadcn/ui` or ad-hoc hardcoded styles unless explicitly chosen in `DESIGN.md`.

## Output Contract
- `docs/project/DESIGN.md` with mapped stack tokens, reference source (library, URL, or MCP extraction), hard floor, and written brand justification for any soft-preference overrides.
- Configured theme provider and exported token files matching `STACK.md`.

## Checklist
- [ ] Requested UI library or brand aesthetic respected (no unrequested `shadcn/ui` default)
- [ ] MCP tools used to fetch real-world tokens when a brand/URL/industry aesthetic is needed
- [ ] Theme provider wired at the app root (when using a UI framework)
- [ ] `docs/project/DESIGN.md` matches every token used in code
- [ ] Hard-floor rules (WCAG AA 4.5:1 contrast, focus states, `prefers-reduced-motion`, mobile fit) satisfied

## Prohibitions
- Assuming `shadcn/ui` or Tailwind defaults when another system or brand aesthetic was requested.
- Guessing brand tokens from memory instead of querying MCP tools or approved references.
- Bypassing dependency approval outside `/auto-build`.


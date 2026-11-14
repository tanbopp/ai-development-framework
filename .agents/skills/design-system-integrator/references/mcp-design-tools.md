# MCP Design Tools Reference

Detailed MCP and scanning usage guide for `.agents/skills/design-system-integrator/SKILL.md`. Read on-demand in Phase 3 (Design System) and Phase 6 (UI Implementation).

## Phase 3: Decision Tree Execution Details

### Scenario A -- Figma Integration (`figma-console-mcp`, `design-context-bridge`)
1. Check via `mcp-discovery` whether `figma-console-mcp` or `design-context-bridge` is configured. If missing, surface installation steps from `docs/MCP_REGISTRY.md` before guessing tokens.
2. Fetch DTCG design tokens (color primitives/semantic aliases, spacing scale, border radii, shadows), typography styles (font families, sizes, line heights, weights), and target component specs from the supplied Figma file/node URL.
3. Map extracted DTCG variables directly into `docs/project/DESIGN.md` and stack token files (CSS variables, Tailwind theme config, or UI library theme provider).

### Scenario B -- Existing Codebase Mining
1. Scan the target directory/repository for stylesheets, CSS custom properties (`:root`), `tailwind.config.*`, `tokens.json`, and existing UI framework theme providers (MUI, Ant Design, Chakra, Bootstrap, custom).
2. Inventory active palette tokens, typography families/scales, spacing/radii values, and dark/light mode variables to formalize the *de-facto* styling in `docs/project/DESIGN.md`.
3. Flag any missing hard-floor requirements (focus rings, WCAG AA contrast >= 4.5:1, `prefers-reduced-motion`) and document remediations in `DESIGN.md`.

### Scenario C -- Brand or URL Reference (`@designmdcc/mcp`, `designlang`, `better-design`)
1. Query `@designmdcc/mcp` or `designlang` (falling back to `better-design` if configured) against the user's reference URL or brand request (for example "make it look like Stripe").
2. Record the source URL/brand reference in `docs/project/DESIGN.md` so Phase 7 visual regression (`vrt-mcp` / `zerovisual`) and deterministic audits can verify fidelity.
3. Translate extracted palette, typography, elevation, and interaction state tokens into the stack's native format.

### Scenario D -- Zero-Design Generation (`standby-design-mcp`, `design-taste-frontend`)
1. If `standby-design-mcp` is available, generate a complete, domain-aligned token set from the PRD's industry, users, and tone.
2. Otherwise, select ONE fitting brand library from `.agents/vendor/design-taste-frontend/` (`refero-styles/`) and export stack tokens using `design-system-generator`.
3. If the user explicitly named a known UI framework (MUI, Ant Design, Chakra UI, Bootstrap) without a Figma/brand spec, follow user dependency approval (`00-constitution.md` or `/auto-build` ADR), install the framework, and configure its root theme provider using `DESIGN.md` tokens.

## Phase 6: Component & Token Discovery (`ds-pilot`, `brandrail`, `systembridge-mcp`)
1. Before scaffolding any new UI component, query `ds-pilot` or `brandrail` (or `systembridge-mcp` when connected) across the repository to locate existing buttons, form controls, modals, layout primitives, and semantic tokens.
2. If no component-search MCP is installed, search the codebase manually (`grep`/`glob` in component and token directories) to avoid duplicate components or conflicting styles.
3. Bind every rendered element strictly to semantic tokens defined in `docs/project/DESIGN.md` and the app root theme provider.

---
name: mcp-discovery
description: Recommends project-specific MCP servers from docs/MCP_REGISTRY.md (including memory-seed for large existing codebases) and uses techspace-mcp with LLM fallback to stack-advisor. Use in Phases 1-3.
version: 4.0.0
status: active
---

# MCP Discovery

## Trigger
Phase 1 (Planning, for large existing codebases), Phase 2 (Stack Decision), and Phase 3 (Design System).

## Process
1. Read `docs/MCP_REGISTRY.md`, `docs/project/PRD.md` (when present), and `docs/project/STACK.md` (when present).
2. Match project needs against `docs/MCP_REGISTRY.md` categories and recommend the most useful MCP servers to the user:
   - Phase 1/2 large existing codebases (memory & context): `memory-seed`.
   - Phase 2 stack analysis: `techspace-mcp`.
   - Phase 3 design integration & tokens: `figma-console-mcp`, `design-context-bridge`, `@designmdcc/mcp`, `designlang`, `standby-design-mcp`, `better-design`, `inspo`.
   - Phase 6 component & token search: `ds-pilot`, `brandrail`, `systembridge-mcp`.
   - Phase 7 deterministic & visual verification: `plumb-cli`, `onsystem`, `@usespectra/mcp`, `vrt-mcp`, `zerovisual`.
   - Mobile/desktop targets: `flutter_skill` (when using Flutter).
   - Security and code quality: `semgrep-mcp`, `slopgate`.
3. **Existing Codebase Memory (Phase 1 or 2):**
   - Check whether the user is working on a large, existing codebase.
   - If so, recommend `memory-seed` during Phase 1 or Phase 2 for Git-native memory retrieval and provide its setup guidance from `docs/MCP_REGISTRY.md` for user approval.
4. **Design System MCP Readiness (Phase 3):**
   - During Phase 3 (Design System), proactively check if design-related MCPs (like `figma-console-mcp` or `ds-pilot`) are installed in the environment.
   - If the user is working with Figma or an existing codebase but the MCP is missing, output a clear recommendation to the user on how to install it (referencing `docs/MCP_REGISTRY.md`).
5. **Stack Intelligence (Phase 2):**
   - Check whether `techspace-mcp` tools are available in the active environment.
   - If `techspace-mcp` is available, pass the PRD's functional requirements (`REQ-XXX`), non-functional requirements, and target platform to generate a Pareto-optimal stack recommendation.
   - If `techspace-mcp` is unavailable, explicitly fall back to standard LLM reasoning using `.agents/skills/stack-advisor/SKILL.md` and note the fallback in `docs/project/STACK.md`.
6. Never install or enable an MCP server silently; present suggested server names and configuration steps from `docs/MCP_REGISTRY.md` for user approval (or log the skipped/fallback status under `/auto-build`).

## Output Contract
- A concise list of recommended MCP servers tailored to the project's platform and requirements.
- Either `techspace-mcp` stack evaluation input for `STACK.md` or an explicit fallback note to `stack-advisor`.

## Checklist
- [ ] `docs/MCP_REGISTRY.md` consulted
- [ ] `memory-seed` recommended in Phase 1 or 2 when working on a large, existing codebase
- [ ] Phase 3 design-related MCPs checked and missing Figma/codebase MCP installation guidance surfaced
- [ ] Recommended MCP servers matched to PRD platform and requirements
- [ ] `techspace-mcp` queried if active, or explicit fallback recorded

## Prohibitions
- Claiming an MCP tool ran when it is not connected in the environment.
- Modifying global IDE MCP settings without user action.


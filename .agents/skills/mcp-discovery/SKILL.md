---
name: mcp-discovery
description: Recommends project-specific MCP servers from docs/MCP_REGISTRY.md and uses techspace-mcp (with LLM fallback to stack-advisor) for Pareto-optimal stack decisions. Use in Phase 2 (Stack Decision) and Phase 3 (Design System).
---

# MCP Discovery

## Trigger
Phase 2 (Stack Decision) and Phase 3 (Design System).

## Process
1. Read `docs/MCP_REGISTRY.md`, `docs/project/PRD.md`, and `docs/project/STACK.md` (when present).
2. Match project needs against `docs/MCP_REGISTRY.md` categories and recommend the most useful MCP servers to the user:
   - Phase 2 stack analysis: `techspace-mcp`.
   - Phase 3 design/tokens: `better-design`, `designlang`, `inspo`.
   - Phase 7 visual verification: `vrt-mcp`, `zerovisual`.
   - Mobile/desktop targets: `flutter_skill` (when using Flutter).
   - Security and code quality: `semgrep-mcp`, `slopgate`.
3. **Stack Intelligence (Phase 2):**
   - Check whether `techspace-mcp` tools are available in the active environment.
   - If `techspace-mcp` is available, pass the PRD's functional requirements (`REQ-XXX`), non-functional requirements, and target platform to generate a Pareto-optimal stack recommendation.
   - If `techspace-mcp` is unavailable, explicitly fall back to standard LLM reasoning using `.agents/skills/stack-advisor/SKILL.md` and note the fallback in `docs/project/STACK.md`.
4. Never install or enable an MCP server silently; present suggested server names and configuration steps from `docs/MCP_REGISTRY.md` for user approval (or log the skipped/fallback status under `/auto-build`).

## Output Contract
- A concise list of recommended MCP servers tailored to the project's platform and requirements.
- Either `techspace-mcp` stack evaluation input for `STACK.md` or an explicit fallback note to `stack-advisor`.

## Checklist
- [ ] `docs/MCP_REGISTRY.md` consulted
- [ ] Recommended MCP servers matched to PRD platform and requirements
- [ ] `techspace-mcp` queried if active, or explicit fallback recorded

## Prohibitions
- Claiming an MCP tool ran when it is not connected in the environment.
- Modifying global IDE MCP settings without user action.


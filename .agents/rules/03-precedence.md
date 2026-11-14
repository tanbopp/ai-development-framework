---
trigger: always_on
---

# Precedence

Tools and skills must not collide. Each has one lane.

1. Design authority: `design-system-applier` is the primary design applier across any UI library or MCP token source, with `design-taste-frontend` as the bundled visual reference generator when no external library or reference is specified.
2. Design audit: Hallmark, unslop, and visual regression (`vrt-mcp` / `zerovisual`) audit only. They never generate new design.
3. Copy audit: text only. Never CSS or components.
4. Code minimalism: Ponytail applies only during implementation, never during design.
5. Pre-commit quality gate: `code-quality-gate` runs before final delivery or `git commit` to reject stubbed functions, silent error handling, unvalidated inputs, and hardcoded secrets.
6. CI gate: `peakoss/anti-slop` applies only to pull-request gates.
7. Project stack and design tokens ALWAYS win over the agent's default preferences.
8. If two rules conflict, the rule with the lower number wins.

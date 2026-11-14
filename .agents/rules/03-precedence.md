---
trigger: always_on
---

# Precedence

Tools and skills must not collide. Each has one lane.

1. Design authority: `design-system-integrator` is the primary design system integrator across Figma, existing codebases, brand/URL references, UI libraries, and MCP token sources, with `standby-design-mcp` or `design-taste-frontend` used when no design reference is specified.
2. Design audit: Hallmark, unslop, and visual regression (`vrt-mcp` / `zerovisual`) audit only. They never generate new design.
3. Copy audit: text only. Never CSS or components.
4. Code minimalism: Ponytail applies only during implementation, never during design.
5. Pre-commit quality gate: `code-quality-gate` runs before final delivery or `git commit` to reject stubbed functions, silent error handling, unvalidated inputs, and hardcoded secrets.
6. CI gate: `peakoss/anti-slop` applies only to pull-request gates.
7. Project stack and design tokens ALWAYS win over the agent's default preferences.
8. If two rules conflict, the rule with the lower number wins.

---
trigger: always_on
---

# Precedence

Tools and skills must not collide. Each has one lane.

1. Design authority: `design-taste-frontend` is the only design generator.
2. Design audit: Hallmark and unslop audit only. They never generate new design.
3. Copy audit: text only. Never CSS or components.
4. Code minimalism: Ponytail applies only during implementation, never during design.
5. CI gate: `peakoss/anti-slop` applies only to pull-request gates.
6. Project stack and design tokens ALWAYS win over the agent's default preferences.
7. If two rules conflict, the rule with the lower number wins.


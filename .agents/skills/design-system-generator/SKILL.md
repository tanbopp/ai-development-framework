---
name: design-system-generator
description: Generates a per-project design system (tokens, typography, spacing, motion) from the PRD tone and approved stack, exported in the stack's format. Use after STACK.md is approved. design-taste-frontend is the only design generator.
version: 4.0.0
status: active
---

# Design System Generator

## Trigger
After `STACK.md` is approved.

## Process
1. Read the PRD, `STACK.md`, and `.agents/rules/02-design-principles.md`.
2. Identify the tone/brand from the PRD (formal, playful, clinical, editorial, etc.).
3. Choose or create consistent tokens: color palette, typography (font, scale, weight hierarchy), spacing, radius, shadow, motion.
4. Export tokens in the format the stack needs (CSS variables, Tailwind config, Style Dictionary, etc.).
5. If adopting a brand library from `.agents/vendor/design-taste-frontend/`, choose exactly ONE library and record the reason in `DESIGN.md`.
6. Write `docs/project/DESIGN.md` and the token files.
7. Stop and request user approval (GATE 3).

## Output Contract
`DESIGN.md` containing full tokens, the hard floor, soft preferences, and a brand justification for every override.

## Checklist
- [ ] Contrast of every text/background pair is at least 4.5:1
- [ ] Three or more typography levels
- [ ] Component states defined (default, hover, focus, disabled, loading, error)
- [ ] Reduced-motion behavior defined
- [ ] Overrides carry written justification

## Prohibitions
- Reusing another project's tokens because it is easier.
- Adding colors or fonts after approval without updating `DESIGN.md`.
- Using Hallmark or unslop to generate design (they audit only).


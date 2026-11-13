---
trigger: model_decision
description: "Apply when generating, reviewing, or auditing UI design. Enforces universal design quality floors and neutral anti-patterns without locking specific tokens or brand aesthetics."
---

# Design Principles

Universal principles. Concrete tokens are generated per project and stored in `docs/project/DESIGN.md`.

1. Design tokens are defined before implementation, stored in `DESIGN.md`, and exported to the format the stack requires.
2. Typography has at least three hierarchy levels (size or weight).
3. Every component defines these states: default, hover, focus, disabled, loading, error.
4. WCAG AA contrast: at least 4.5:1 for normal text.
5. Motion respects `prefers-reduced-motion`.
6. Do not reuse another project's tokens because it is easier.

## Hard floor (cannot be overridden)

- WCAG AA contrast failures
- Missing focus state
- Ignored `prefers-reduced-motion`
- Text truncated or overflowing on mobile

## Soft preferences (may be overridden with written brand justification in `DESIGN.md`)

- Default purple/indigo/violet accent without a brand reason
- Purple-to-blue gradient as the default hero
- Inter, Roboto, or Arial as the sole typographic identity
- Identical three-card grid with icons in rounded squares
- Glassmorphism on every surface
- `hover:scale-105` on every card

An override is valid only if `DESIGN.md` names the preference, the reason, and the brand context.


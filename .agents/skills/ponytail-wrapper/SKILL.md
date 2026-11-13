---
name: ponytail-wrapper
description: Applies Ponytail, a structured YAGNI guardrail, to every implementation task with lite, full, or ultra intensity while preserving security guardrails. Use during implementation only, never during design.
---

# Ponytail Wrapper

## Trigger
Every implementation task (frontend or backend).

## Process
1. Read the task and the code it touches. List every place the change must reach: callers, tests, fixtures, config, exports.
2. Run the decision ladder and stop at the first rung that fully works:
   1. Does it need to exist?
   2. Does it already exist in this codebase?
   3. Can the standard library do it?
   4. Can a platform-native feature do it?
   5. Can an installed dependency do it?
   6. Can it be one readable line?
   7. Only then: write the minimum code.
3. Choose intensity: `lite` for trivial tasks (or disable), `full` for medium-to-large tasks, `ultra` only for refactors and code reduction.
4. Preserve security: validate at trust boundaries, keep error handling that prevents data loss, keep accessibility.
5. End the reply with one or two lines: what was skipped or not checked, and any risk.

Upstream: `.agents/vendor/ponytail/` (see `init-project` workflow). Do not modify vendor files.

## Output Contract
Minimal code that passes tests and audits.

## Checklist
- [ ] Ladder run before writing code
- [ ] Mode matches task size
- [ ] Non-trivial new logic has a small test
- [ ] Trust-boundary validation intact

## Prohibitions
- Sacrificing security for brevity.
- Using `ultra` on unstable new features.

## Honest note
Ponytail is structured YAGNI. It helps against overengineering on medium-to-large tasks. For trivial tasks use `lite` or turn it off. Upstream benchmark figures (for example reduced lines of code) come from specific setups and are not a universal guarantee. Do not promise them.


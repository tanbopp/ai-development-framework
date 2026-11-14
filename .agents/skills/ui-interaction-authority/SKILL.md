---
name: ui-interaction-authority
description: Enforces designated headless UI primitives, portal rendering, collision detection, and single-library consistency in Phase 6 while banning unapproved native form and dialog primitives.
version: 6.0.0
status: active
---

# UI Interaction Authority

## Trigger
Phase 6 (Implementation) -- mandatory before generating any form, input, dropdown, date picker, combobox, popover, modal, or toast.

## Process
1. **Identify Designated Headless Library:**
   - Read `docs/project/STACK.md` and `docs/project/DESIGN.md` to identify the approved headless UI library (for example, Radix UI, Base UI, or Ark UI).
2. **Ban Native Interactive Primitives for Core UI:**
   - Native `<select>`, `<input type="date">`, `<input type="time">`, `alert()`, `confirm()`, and `prompt()` are strictly forbidden unless explicitly approved in `docs/project/DESIGN.md` with a written justification.
3. **Enforce Portal Rendering:**
   - Every popover, dropdown menu, select, date picker, and combobox MUST use `portal={true}` (or the library's portal wrapper such as `<Select.Portal>` / `<Popover.Portal>`) so floating content renders inside `document.body` and escapes parent `overflow: hidden` containers.
4. **Enforce Collision Detection:**
   - Floating elements MUST use a positioning engine (Floating UI or the designated library's built-in collision engine) with `flip`, `shift`, and `autoUpdate` enabled.
5. **Enforce Single Library Consistency:**
   - Never mix headless UI libraries within one component tree (for example, a Radix dropdown inside a Base UI modal). Pick the single library specified in `docs/project/STACK.md` and use it consistently.
6. **Record Component Compliance:**
   - Write and maintain `docs/project/INTERACTION_AUDIT.md` listing every interactive component, its designated primitive, its portal configuration, and its collision-detection setup.

## Output Contract
`docs/project/INTERACTION_AUDIT.md` listing every interactive UI component and confirming designated-library primitive usage, `portal={true}` (or equivalent) rendering, collision detection (`flip`, `shift`, `autoUpdate`), and single-library consistency.

## Checklist
- [ ] Designated headless UI library identified from `docs/project/STACK.md`
- [ ] Zero unapproved native `<select>`, `<input type="date">`, `<input type="time">`, `alert()`, `confirm()`, or `prompt()` calls
- [ ] `portal={true}` (or equivalent) verified on every dropdown, select, date picker, popover, and combobox
- [ ] Positioning engine collision detection (`flip`, `shift`, `autoUpdate`) enabled on every floating element
- [ ] Single headless UI library maintained across all component trees
- [ ] All interactive components logged in `docs/project/INTERACTION_AUDIT.md`

## Prohibitions
- Defaulting to native browser `<select>`, `<input type="date">`, `<input type="time">`, `alert()`, `confirm()`, or `prompt()` without explicit justification in `docs/project/DESIGN.md`.
- Rendering popovers, selects, or date pickers inline inside clipped containers without a portal and collision detection.
- Mixing competing headless primitive libraries inside the same application component tree.

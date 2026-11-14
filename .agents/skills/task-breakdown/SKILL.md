---
name: task-breakdown
description: Breaks approved technical specs into small, verifiable vertical-slice tasks with IDs, files_allowed, binary acceptance_criteria, dependencies, and estimates in docs/tasks/TASKS.md. Use after technical specs are approved.
version: 4.0.0
status: active
---

# Task Breakdown

## Trigger
After the technical specs are approved.

## Process
1. Read `docs/specs/*.md` and `ARCHITECTURE.md`.
2. **Vertical Slicing (Strict Rule):** You must use Vertical Slicing. A task must include UI, Logic, and Data changes together to deliver a functional slice. Do not group tasks by technical layer (e.g., 'all database tasks', then 'all UI tasks').
3. Split the work into small, verifiable tasks. Each task has: ID, description, `files_allowed` array, binary `acceptance_criteria`, dependencies, estimate, and the `REQ-XXX` it serves.
4. **Hard Requirements:**
   - Every task MUST have a `files_allowed` array and a binary `acceptance_criteria`.
   - If a project has more than 5 features, you MUST generate at least 15 tasks.
5. Order tasks by dependency.
6. Write `docs/tasks/TASKS.md` as a tickable checklist.

## Output Contract
`docs/tasks/TASKS.md` with a checklist of vertical-slice tasks (including `files_allowed` and binary `acceptance_criteria`) that can be ticked off.

## Checklist
- [ ] Tasks use Vertical Slicing (UI + Logic + Data together per functional slice)
- [ ] Every task has a `files_allowed` array and binary `acceptance_criteria`
- [ ] At least 15 tasks generated when the project has more than 5 features
- [ ] Every task is reviewable in one sitting
- [ ] Every `REQ-XXX` is covered by at least one task

## Prohibitions
- Grouping tasks by technical layer (such as all database tasks before all UI tasks).
- Tasks too large to review.
- Tasks without a `files_allowed` array or binary `acceptance_criteria`.


---
name: task-breakdown
description: Breaks approved technical specs into small, verifiable tasks with IDs, files touched, acceptance criteria, dependencies, and estimates in docs/tasks/TASKS.md. Use after technical specs are approved.
---

# Task Breakdown

## Trigger
After the technical specs are approved.

## Process
1. Read `docs/specs/*.md` and `ARCHITECTURE.md`.
2. Split the work into small, verifiable tasks. Each task has: ID, description, files touched, acceptance criteria, dependencies, estimate, and the `REQ-XXX` it serves.
3. Order tasks by dependency.
4. Write `docs/tasks/TASKS.md` as a tickable checklist.

## Output Contract
`docs/tasks/TASKS.md` with a checklist that can be ticked off.

## Checklist
- [ ] Every task has acceptance criteria
- [ ] Every task is reviewable in one sitting
- [ ] Every `REQ-XXX` is covered by at least one task

## Prohibitions
- Tasks too large to review.
- Tasks without acceptance criteria.


---
name: boundary-guard
description: Restricts Phase 6 file modifications strictly to paths listed in the active task's files_allowed field in docs/tasks/TASKS.md, halting on missing scopes or unauthorized edits.
version: 4.0.0
status: active
---

# Boundary Guard

## Trigger
Mandatory before any file creation, edit, or deletion during Phase 6 (Implementation).

## Process
1. Read the active task in `docs/tasks/TASKS.md` and locate its `files_allowed` field.
2. **If `files_allowed` is missing or empty:**
   - **HALT** immediately without editing any source file.
   - Request updating `docs/tasks/TASKS.md` so the task explicitly defines its `files_allowed` paths before implementation continues.
3. **Check Target File Path:**
   - Before creating, modifying, or deleting any file, normalize its workspace-relative path and check whether it appears explicitly in the active task's `files_allowed` list.
4. **Halt on Out-of-Scope File:**
   - If the target file is NOT listed in `files_allowed`, **STOP** immediately.
   - Ask the user for permission to expand the task scope, explaining why the extra file needs changes, and update `files_allowed` in `docs/tasks/TASKS.md` only after approval.

## Output Contract
Every file touched during Phase 6 is explicitly authorized in the active task's `files_allowed` field (or approved for scope expansion by the user).

## Checklist
- [ ] Active task in `docs/tasks/TASKS.md` defines `files_allowed`
- [ ] Every target file path verified against `files_allowed` prior to editing
- [ ] Out-of-scope edits halted for explicit user permission

## Prohibitions
- Never modify files outside the active task's `files_allowed` scope.
- Never silently expand `files_allowed` without user confirmation.

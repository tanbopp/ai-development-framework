---
name: prompt-history
description: Creates immutable Prompt History Records in docs/history/PHR-[YYYYMMDD-HHMM].md at phase completions, major task finishes, and workflow endings to preserve an audit trail of prompts, changes, tests, and decisions.
version: 4.1.0
status: active
---

# Prompt History Record (PHR)

## Trigger
Every time a pipeline phase is completed, a major task is finished, or at the end of any workflow.

## Process
1. Determine the current local timestamp (`YYYYMMDD-HHMM`) and create a new file at `docs/history/PHR-[YYYYMMDD-HHMM].md` (if a record for that exact minute already exists from the same session phase sequence, append a new timestamped sub-entry or sequence suffix so nothing is overwritten).
2. Write the following structured Prompt History Record:
   ```markdown
   # Prompt History Record
   - **Phase:** [Phase Name]
   - **User Goal:** [Brief summary]
   - **Files Modified:** [List of files]
   - **Tests Run:** [Commands executed]
   - **Key Decisions:** [Any autonomous decisions made]
   ```
3. Preserve all previously created `docs/history/PHR-*.md` records untouched so `docs/history/` serves as an immutable audit trail of the agent's execution and reasoning history.

## Output Contract
An immutable markdown record written to `docs/history/PHR-[YYYYMMDD-HHMM].md` documenting phase execution, modified files, executed checks, and key decisions.

## Checklist
- [ ] File created at `docs/history/PHR-[YYYYMMDD-HHMM].md`
- [ ] `Phase`, `User Goal`, `Files Modified`, `Tests Run`, and `Key Decisions` populated from verified session actions
- [ ] Existing PHR files in `docs/history/` left unmodified

## Prohibitions
- Deleting, rewriting, or editing earlier historical PHR logs.
- Fabricating commands or files modified that were not actually run or changed in the session.

---
name: evidence-gated-blocking
description: Blocks Phase 6 completion, Phase 7 completion, and Phase 8 delivery unless raw, unedited command verification logs exist in docs/tasks/logs/[phase-name].log with zero errors and exit code 0.
version: 4.0.0
status: active
---

# Evidence-Gated Blocking

## Trigger
Mandatory at the end of Phase 6 (Implementation) and Phase 7 (Audit), and enforced as a pre-condition before Phase 8 (Delivery).

## Process
1. **No Self-Certification:**
   - A phase CANNOT be marked complete simply because the agent believes edits or tests are finished.
2. **Capture Raw Execution Logs:**
   - Run the concrete verification commands for the current phase (for example `npm run build`, `npm test`, `tsc --noEmit`, lint scripts, or stack equivalents).
   - Capture the raw, unedited terminal output together with the final exit status and save it to `docs/tasks/logs/[phase-name].log` (for example `docs/tasks/logs/phase-6.log` and `docs/tasks/logs/phase-7.log`).
3. **Block on Failure Signals:**
   - Inspect the saved `.log` file. If it contains any `error`, failed test/check, or non-zero exit status (such as `exit code 1`), the phase status is **BLOCKED**.
   - Return to implementation, fix the underlying defect, re-run verification from scratch, and overwrite the log with the fresh unedited output.
4. **Strict Phase 8 Delivery Gate:**
   - Transitioning to Phase 8 (Delivery) is strictly forbidden unless both `docs/tasks/logs/phase-6.log` and `docs/tasks/logs/phase-7.log` (or the active phase log) exist on disk and explicitly show `exit code 0` with zero errors.

## Output Contract
Unedited terminal output files stored at `docs/tasks/logs/[phase-name].log` demonstrating `exit code 0` prior to marking a phase complete or starting Phase 8.

## Checklist
- [ ] Verification commands actually executed after final edits (not recalled from earlier runs)
- [ ] Raw, unedited stdout/stderr and exit code saved to `docs/tasks/logs/[phase-name].log`
- [ ] Zero errors and `exit code 0` verified inside the saved log file
- [ ] Phase 8 blocked until valid Phase 6 and Phase 7 evidence logs exist

## Prohibitions
- Marking Phase 6 or Phase 7 done without a saved `docs/tasks/logs/[phase-name].log` file.
- Editing, truncating, filtering out failures, or fabricating log contents.
- Proceeding to Phase 8 (Delivery) when any log contains `error` or a non-zero exit code.

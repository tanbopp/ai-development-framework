---
name: snapshot-rollback
description: Creates clean git snapshot commits before Phase 6 and after major features, logs commit hashes in docs/project/PREFLIGHT.md, and restores state with git reset --hard on rollback requests.
version: 4.0.0
status: active
---

# Snapshot Rollback

## Trigger
Mandatory right before starting Phase 6 (Implementation) and after completing major features.

## Process
1. Check whether Git is initialized in the workspace (`git rev-parse --is-inside-work-tree`).
   - If Git is not initialized, run `git init` (and ensure `.gitignore` excludes `.env`, `node_modules/`, and build artifacts).
2. Stage and commit the current state as a clean recovery snapshot:
   - Run `git add -A` and `git commit -m "chore: auto-snapshot before Phase 6/feature"`.
   - If the working tree has no uncommitted changes to commit, keep the clean working tree and use the current `HEAD` commit hash.
3. Record the snapshot commit hash, timestamp, and checkpoint label (for example `pre-phase-6` or `after-<feature>`) in `docs/project/PREFLIGHT.md`.
4. **Rollback Procedure:**
   - If the user requests a rollback due to an AI failure or unwanted regression, confirm the target snapshot `<hash>` from `docs/project/PREFLIGHT.md` and run `git reset --hard <hash>`.

## Output Contract
A clean Git working tree and a recorded recovery commit hash in `docs/project/PREFLIGHT.md`.

## Checklist
- [ ] Git repository initialized and `.gitignore` verified
- [ ] Snapshot committed (`chore: auto-snapshot before Phase 6/feature`) or clean `HEAD` verified
- [ ] Commit hash logged in `docs/project/PREFLIGHT.md`

## Prohibitions
- Starting Phase 6 edits without a clean snapshot commit recorded first.
- Running `git reset --hard` without a user rollback request.

# Workflow: /transactional-wave

Trigger: Manual invocation by advanced users (for example `/transactional-wave`).

## Purpose
Runs parallel AI agents on independent features simultaneously using isolated Git worktrees (`git worktree add`) without file collisions or dirty working-tree conflicts, then merges verified waves back atomically. This is an advanced workflow intended for large-scale projects after Phase 5 (`docs/tasks/TASKS.md`) completes.

## Prerequisites
- Phases 0 to 5 completed (`PREFLIGHT.md`, `PRD.md`, `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, and vertical-slice `docs/tasks/TASKS.md` with explicit non-overlapping `files_allowed` lists per wave).
- Clean Git working tree (`snapshot-rollback` baseline commit recorded in `docs/project/PREFLIGHT.md`).

## Process

### 1. Partition Independent Waves
1. Select tasks from `docs/tasks/TASKS.md` that have no blocking dependencies between them and whose `files_allowed` scopes are completely disjoint.
2. Shared contract or schema files must finish in an earlier sequential wave before parallel feature waves branch from it.

### 2. Create Isolated Worktrees per Feature Wave
For each parallel feature `<feature>` assigned to its own branch `feat/<feature>`:

```bash
# Run from the repository root on a clean baseline branch
git worktree add ../wt-<feature> -b feat/<feature>
```

PowerShell equivalent:
```powershell
git worktree add ..\wt-<feature> -b feat/<feature>
```

### 3. Assign One Agent per Worktree
1. Launch a separate agent session inside each worktree directory (for example `../wt-auth`, `../wt-billing`).
2. Each agent MUST:
   - Read `AGENTS.md`, `.agents/MEMORY.md`, and `.agents/LESSONS.md`.
   - Enforce `boundary-guard` strictly so edits remain inside its assigned task's `files_allowed`.
   - Run `context-grounding` before writing imports and `evidence-gated-blocking` to save passing logs (`docs/tasks/logs/phase-6-<feature>.log`).

### 4. Verify & Merge Atomically
Merge one completed worktree branch at a time only after its local tests, `code-quality-gate`, and `security-hardening` checks pass:

```bash
# Return to the main integration worktree
git checkout main

# Run an atomic merge commit per verified feature wave
git merge --no-ff feat/<feature> -m "wave(<feature>): integrate verified vertical slice"

# Re-run full integration build and test suite before merging the next wave
npm test && npm run build
```

If a merge conflict or post-merge test failure occurs, abort (`git merge --abort`) or fix inside a fresh remediation commit before integrating the next wave. Run `lessons-learned` if any failure surfaces.

### 5. Clean Up Worktrees & Finalize Audit
```bash
git worktree remove ../wt-<feature>
git branch -d feat/<feature>
git worktree prune
```

Once all waves are integrated into the main branch, run the complete Phase 7 (`design-audit`, `copy-audit`, `code-quality-gate`, `security-hardening`, `test-suite-auditor`, `meta-judge`, `evidence-gated-blocking`) before Phase 8 delivery.

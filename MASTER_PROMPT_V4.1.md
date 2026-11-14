# MASTER PROMPT V4.1 — THE ULTIMATE INTEGRATION

You are a Principal AI Systems Architect. Your task is to implement the final upgrade to the V4 framework: **Version 4.1 (The Ultimate Integration)**. 

We are adopting the best features from the most advanced open-source SDD frameworks (Spec Kit, Kerrigan, Agentic-SDLC, Spec Kit Plus). This phase focuses on cross-artifact consistency, two-axis reviews, immutable audit trails, and code size limits.

Execute the following tasks exactly. Do not skip steps. Write all files in English. Keep all existing V3 and V4 Phase 1-5 logic intact. Do not push to GitHub.

## TASK 1: Artifact Consistency Analyzer (from Spec Kit)
Create a new skill `.agents/skills/artifact-analyzer/SKILL.md`.
- **Trigger:** Phase 4 (between Technical Spec and Task Breakdown).
- **Process:**
  1. Read `PRD.md`, `ARCHITECTURE.md`, and `docs/specs/*.md`.
  2. Perform a read-only semantic analysis to detect:
     - **Ambiguity:** Requirements that are unclear.
     - **Inconsistency:** E.g., PRD says PostgreSQL, but Architecture says MongoDB.
     - **Duplication:** Overlapping requirements.
     - **Constitution Violations:** Any spec that violates `.agents/rules/00-constitution.md`.
  3. If any CRITICAL issue is found, **BLOCK** progression to Phase 5. Output a list of findings and instruct the AI to fix the specs.
  4. Save the analysis report to `docs/project/ANALYSIS.md`.

## TASK 2: Two-Axis Review (from Agentic-SDLC)
Update `.agents/skills/meta-judge/SKILL.md` and `.agents/skills/design-audit/SKILL.md`.
- **Process:** At the end of the audit, the output MUST be split into two distinct sections:
  - **Axis 1: Agent Fix List:** Technical findings that the AI MUST fix automatically (e.g., WCAG contrast < 4.5, missing `alt` tags, TypeScript `any`, failed unit tests).
  - **Axis 2: Human Decision List:** Findings that require human intent (e.g., "Is this UX flow intuitive?", "Should this API be public?", "Trade-off between speed and battery").
- **Output Contract:** The audit report must explicitly state: "Agents will fix Axis 1. The human must review Axis 2."

## TASK 3: Prompt History Records (from Spec Kit Plus)
Create a new skill `.agents/skills/prompt-history/SKILL.md`.
- **Trigger:** Every time a phase is completed or a major task is finished.
- **Process:**
  1. Create a file in `docs/history/PHR-[YYYYMMDD-HHMM].md`.
  2. Append the following structured log:
     ```markdown
     # Prompt History Record
     - **Phase:** [Phase Name]
     - **User Goal:** [Brief summary]
     - **Files Modified:** [List of files]
     - **Tests Run:** [Commands executed]
     - **Key Decisions:** [Any autonomous decisions made]
This creates an immutable audit trail of the AI's thought process.

Update AGENTS.md to mandate: "At the end of every workflow, run prompt-history to log the session."

TASK 4: Code Quality Bar & Architecture Guard (from Kerrigan & Spec Kit)
Update .agents/skills/code-quality-gate/SKILL.md.

Add a strict rule: "The 800 LOC Limit." If the AI writes more than 800 lines of code in a single task/PR, it MUST stop, split the code into smaller logical chunks, or ask the user to break the task down. This prevents massive, unreviewable PRs.

Create a new skill .agents/skills/architecture-guard/SKILL.md.

Trigger: Phase 7 (Audit).

Process: Read ARCHITECTURE.md and compare it with the actual folder structure and imports in the codebase. If the AI introduced a dependency or created a folder that violates the architecture (e.g., a UI component importing directly from the database layer), flag it as an Architectural Drift and demand a refactor.

TASK 5: Wire Everything Together
Update .agents/workflows/full-cycle.md and .agents/workflows/auto-build.md:

Add artifact-analyzer between Phase 4 and Phase 5.

Add architecture-guard and prompt-history to Phase 7.

Update the output format in Phase 7 to reflect the "Two-Axis Review".

Update .agents/workflows/init-project.md readiness checklist to include the new skills (total 26 skills).

Update README.md to list the new V4.1 features.

VERIFICATION & CONSTRAINTS
Report the following:

The exact content of .agents/skills/artifact-analyzer/SKILL.md.

The exact content of the updated meta-judge/SKILL.md (focusing on the Two-Axis Review).

Confirmation that the 800 LOC limit is added to code-quality-gate.

Confirmation of no non-ASCII characters.

Stop and wait for my review.

CONSTRAINTS:

All files must be in English.

Do not overwrite existing V4 logic.

Do not write application code.

Do not push to GitHub.
---
name: human-in-the-loop
description: Requires explicit user approval during Phase 6 and Phase 7 before modifying critical files (auth, migrations, payments, .env) or proceeding when implementation confidence is low.
version: 4.0.0
status: active
---

# Human In The Loop

## Trigger
Mandatory during Phase 6 (Implementation) and Phase 7 (Audit).

## Process
1. **Critical File Detection:** Before modifying or creating any file, check whether it belongs to a critical category:
   - Authentication, authorization, session, or RLS security policies
   - Database schemas and migrations
   - Payment, billing, or financial transaction logic
   - Environment, secrets, CI/CD, or security configuration files (`.env*`, `.github/workflows/`)
   If the file is critical, **STOP** immediately before editing.
2. **Low-Confidence Gate:** If implementation confidence is low (ambiguous edge cases, unclear side effects, risky refactors, or repeated failed fix attempts), **STOP** immediately instead of guessing.
3. **Request Explicit Approval:** Prompt the user clearly using this format:
   `"I need to modify [file], which is a critical file. Do you approve this change?"`
   Include a brief summary of the intended change and risk, and wait for explicit approval before touching the file.

## Output Contract
Explicit user approval obtained before modifying any critical file or proceeding on a low-confidence change.

## Checklist
- [ ] Target file screened against critical categories (auth, migrations, payments, `.env`)
- [ ] Implementation confidence verified before proceeding
- [ ] Explicit user approval received for every critical-file edit

## Prohibitions
- Never modify critical files autonomously without user confirmation.
- Never guess or force through a low-confidence change silently.

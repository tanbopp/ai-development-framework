---
name: code-quality-gate
description: Pre-commit and pre-delivery gate that scans newly generated or modified code for AI failure modes including stubbed functions, silent error handling, unvalidated inputs, and hardcoded secrets. Use before final delivery or git commit.
---

# Code Quality Gate

## Trigger
Before final delivery (Phase 7/8) or before running `git commit`.

## Process
Scan every newly generated or modified source file in scope for the following AI failure modes:

1. **Stubbed or unimplemented logic:**
   - Look for `throw new Error('Not implemented')`, `throw new NotImplementedError`, `TODO` / `FIXME` / `XXX` placeholders standing in for real behavior, empty function bodies, or hardcoded mock return data left in production paths.
2. **Silent error handling:**
   - Look for empty catch blocks (`catch (e) {}`, `except: pass`, ignored returned errors) or catch handlers that swallow failures without logging, user-facing error states, recovery, or rethrowing.
3. **Unvalidated user inputs:**
   - Check all trust boundaries (HTTP/API handlers, form submissions, CLI arguments, file path operations, dynamic queries) to verify schema/type validation, bounds checking, and safe parameterization.
4. **Hardcoded secrets or credentials:**
   - Scan for API keys, tokens, passwords, private keys, or connection strings with embedded credentials. All secrets must come from environment variables or a secret manager and never be committed.

If **any** item above is found:
- Do NOT proceed to delivery or `git commit`.
- Fix every finding in the code (or update tests/env handling), re-scan, and verify all tests pass before continuing.

## Output Contract
A pass/fail scan report listing checked categories, `file:line` findings (if any), and confirmation that all findings were resolved before commit or delivery.

## Checklist
- [ ] Zero stubbed or `Not implemented` production functions
- [ ] Zero silent/empty `catch` or swallowed error paths
- [ ] All trust-boundary inputs validated
- [ ] Zero hardcoded secrets, tokens, or API keys
- [ ] Existing unit and E2E tests pass after fixes

## Prohibitions
- Allowing stubbed handlers or empty `catch` blocks to ship as "temporary".
- Skipping trust-boundary validation for brevity.
- Committing `.env` values or inline credentials.


---
name: test-suite-auditor
description: Audits unit, integration, and E2E test suites in Phase 7 using a mutation-testing mindset to reject shallow happy-path-only tests and enforce meaningful edge-case and failure-path coverage.
version: 4.0.0
status: active
---

# Test Suite Auditor

## Trigger
Mandatory during Phase 7 (Audit), alongside `code-quality-gate` and `security-hardening`.

## Process
1. **Inspect Phase 6 Tests:**
   - Read all unit, integration, and E2E test files created or updated during Phase 6 and compare them against `docs/project/PRD.md` EARS Acceptance Criteria and `docs/specs/*.md`.
2. **Apply Mutation-Testing Mindset:**
   - For each critical module, handler, or UI workflow, ask: *"If I intentionally break the core logic, will this test catch it?"*
   - Reject weak assertions (such as testing only `toBeDefined()`, tautological mocks, mocked-out units under test, or snapshot-only assertions where behavioral state changes should be verified).
3. **Verify Edge-Case & Failure-Path Coverage:**
   - Confirm tests explicitly exercise failure modes and boundary conditions, not only the "happy path":
     - Empty, null, malformed, or boundary inputs (schema validation rejection).
     - Network failures, timeouts, database errors, or missing resources.
     - Unauthorized/unauthenticated access and cross-tenant/IDOR attempts where auth applies.
4. **Enforce Reject-and-Fix Loop:**
   - If the test suite is judged shallow, relies on fake coverage, or omits edge/failure cases for any `REQ-XXX`, **REJECT** the Phase 7 audit and return to Phase 6 with a concrete list of missing tests to implement before re-auditing.

## Output Contract
A Phase 7 test audit verdict listing verified behavioral/edge-case tests per requirement and either an explicit PASS or a REJECT list requiring additional tests in Phase 6.

## Checklist
- [ ] Core logic assertions survive a mutation-testing review ("if core logic breaks, test fails")
- [ ] Empty/invalid input edge cases covered
- [ ] Network, timeout, or runtime failure paths covered
- [ ] Unauthorized access / permission edge cases covered where applicable
- [ ] Shallow or happy-path-only suites rejected back to Phase 6

## Prohibitions
- Approving a test suite that only exercises happy paths or mocks away the logic being tested.
- Passing Phase 7 with skipped (`test.skip` / `it.skip`) or empty test cases.

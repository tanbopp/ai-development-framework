# EARS Acceptance Criteria Reference

All Acceptance Criteria (AC) in `docs/project/PRD.md` and `docs/specs/PRD-*.md` MUST follow the Easy Approach to Requirements Syntax (EARS). Write single, unambiguous, testable statements using the five templates below.

## Five EARS Templates

1. **Ubiquitous:** Always active invariant or system property.
   - Format: `The system shall [behavior].`
   - Example: `The system shall store all uploaded files encrypted at rest using AES-256.`

2. **Event-driven:** Response triggered when a specific event occurs.
   - Format: `WHEN [trigger], THE SYSTEM SHALL [behavior].`
   - Example: `WHEN a user submits a valid login form, THE SYSTEM SHALL issue a session token and redirect to /dashboard within 500 ms.`

3. **State-driven:** Behavior enforced while a condition or state holds.
   - Format: `WHILE [state], THE SYSTEM SHALL [behavior].`
   - Example: `WHILE the application is in offline mode, THE SYSTEM SHALL queue mutations locally and display an offline indicator.`

4. **Unwanted behavior:** Handling of errors, invalid inputs, failures, or boundary violations.
   - Format: `IF [condition], THEN THE SYSTEM SHALL [behavior].`
   - Example: `IF an API request carries an expired token, THEN THE SYSTEM SHALL return HTTP 401 without disclosing stack traces.`

5. **Optional feature:** Behavior gated by configuration, plan tier, or platform capability.
   - Format: `WHERE [feature is included], THE SYSTEM SHALL [behavior].`
   - Example: `WHERE audit export is enabled for workspace admins, THE SYSTEM SHALL allow downloading compliance logs in CSV and JSON formats.`

## Writing Rules

- Avoid vague terms such as `fast`, `user-friendly`, `seamless`, `secure`, or `as appropriate` without measurable criteria.
- Every EARS statement must map cleanly to a binary pass/fail verification check in Phase 7.

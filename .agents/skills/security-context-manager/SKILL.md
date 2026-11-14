---
name: security-context-manager
description: Collaborates in Phase 1 and Phase 3 using .agents/security-context.yaml (and optional Aegis MCP) to produce a project-specific threat model at docs/project/THREAT_MODEL.md.
version: 5.0.0
status: active
---

# Security Context Manager

## Trigger
Phase 1 (Planning) and Phase 3 (Design).

## Process
1. Read `.agents/security-context.yaml`, `docs/project/PRD.md`, and `docs/project/STACK.md` (when available).
2. Collaborate with the user (or derive from PRD/NFRs under `/auto-build` with explicit ADR recording) to fill out every section of the threat model template:
   - `trust_boundaries` (for example client-to-API, API-to-database, IPC, webhooks)
   - `data_classifications` (`PII`, `PHI`, `PCI`, `internal`, `public`)
   - `authentication_required` and authorization/IDOR rules
   - `input_validation_standard` (for example Zod, Joi, Pydantic, Rust `serde`)
   - `output_encoding_required` and safe error handling
   - `secrets_management` and `compliance_standards` (OWASP ASVS, SOC2, HIPAA, etc.)
3. If Aegis MCP is available, call `aegis_assess` to validate threat model completeness during planning/design, and `aegis_review` to verify generated code adheres to the threat model.
4. Save the finalized project threat model to `docs/project/THREAT_MODEL.md`.

## Output Contract
`docs/project/THREAT_MODEL.md` containing a complete, project-specific threat model covering all fields defined in `.agents/security-context.yaml`.

## Checklist
- [ ] `.agents/security-context.yaml` schema fields all evaluated and populated
- [ ] PII/PHI/PCI data flows, trust boundaries, and validation standards documented
- [ ] `aegis_assess` / `aegis_review` invoked when Aegis MCP is connected
- [ ] Finalized threat model saved to `docs/project/THREAT_MODEL.md`

## Prohibitions
- Proceeding past Phase 3 without a project-specific `docs/project/THREAT_MODEL.md`.
- Assuming "no authentication or validation needed" when user data or network endpoints exist.

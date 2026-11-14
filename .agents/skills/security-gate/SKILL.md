---
name: security-gate
description: Pre-delivery binary security gate in Phase 7/8 that invokes security-mcp to autonomously fix SAST/SCA issues (and re-verify), blocks Phase 8 on any HIGH/CRITICAL finding, and writes the Security verdict to docs/tasks/AUDIT.md.
version: 5.0.0
status: active
---

# Security Gate

## Trigger
Phase 7 (Audit) and mandatory pre-delivery checkpoint before Phase 8 (Delivery).

## Process
1. **Aggregate All Security Evidence:**
   - Inspect `docs/project/THREAT_MODEL.md`, `sast-scanner` logs (`docs/tasks/logs/sast-*.log`), `sca-secret-scanner` logs (`docs/tasks/logs/sca-*.log`), `dast-scanner` logs (`docs/tasks/logs/dast-*.log`), `security-hardening`, `agentic-security-guard`, and `Sabba` finding proofs (if connected).
2. **Autonomous Remediation (`security-mcp`):**
   - If `security-mcp` is available and open SAST or SCA findings remain, invoke `security-mcp` to autonomously fix the findings directly in the working tree (honoring `boundary-guard` scope and `human-in-the-loop` confirmation for critical auth, migration, or payment files).
   - Immediately re-run the affected SAST/SCA scans after fixes to verify the vulnerabilities are cleared.
3. **Enforce Binary Pre-Delivery Gate:**
   - Check whether **any** `HIGH` or `CRITICAL` finding, exposed secret, or unresolved critical threat-model violation remains.
   - If **any** `HIGH` or `CRITICAL` finding remains: **BLOCK Phase 8 (Delivery)** (`FAIL`), classify items under Axis 1 (or Axis 2 if architectural/intent tradeoff), and loop back to Phase 6.
4. **Record Final Verdict:**
   - Write the consolidated security gate verdict, referenced log paths, and pass/block status to `docs/tasks/AUDIT.md` under a **`## Security`** section.

## Output Contract
A binary `PASS` or `BLOCKED` **`## Security`** section written to `docs/tasks/AUDIT.md`; Phase 8 (Delivery) is permitted only when zero `HIGH` or `CRITICAL` findings remain.

## Checklist
- [ ] Threat model, SAST, SCA/secrets, DAST, and agentic security outputs verified
- [ ] `security-mcp` used for autonomous fixes and re-scanned when available
- [ ] Zero `HIGH` or `CRITICAL` findings remain (binary pass/fail gate)
- [ ] Final security verdict written under `## Security` in `docs/tasks/AUDIT.md`

## Prohibitions
- Allowing Phase 8 delivery when any `HIGH` or `CRITICAL` finding, leaked secret, or failed SAST/SCA log remains.
- Claiming `security-mcp` cleared an issue without re-running the verification scan.

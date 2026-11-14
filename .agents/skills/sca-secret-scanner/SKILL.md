---
name: sca-secret-scanner
description: Scans for hardcoded secrets and dependency CVEs/SBOM risks (using GoThreatScope, ironward, @infoinlet/mcp-codeaudit, or GitHub secret scanning), saving logs to docs/tasks/logs/sca-[timestamp].log and blocking on leaks or CRITICAL CVEs.
version: 5.0.0
status: active
---

# SCA & Secret Scanner

## Trigger
Phase 6 (Implementation) and Phase 7 (Audit).

## Process
1. **Secret Scanning:**
   - Scan the full codebase and configuration files for hardcoded secrets (API keys, OAuth tokens, passwords, JWT secrets, private keys, connection strings).
   - Use `GoThreatScope` (which integrates Gitleaks) or `ironward` MCP if available, or `GitHub MCP Server` secret scanning when hosted on GitHub. If none are configured, run local secret-pattern checks / Gitleaks CLI and document the method used.
2. **Dependency Scanning (SBOM & SCA):**
   - Generate an SBOM across the project's manifests and lockfiles and check all dependencies against OSV.dev for known CVEs and malicious packages.
   - Use `GoThreatScope` or `@infoinlet/mcp-codeaudit` MCP when available (falling back to stack-native audit CLIs such as `npm audit`, `pnpm audit`, `cargo audit`, `govulncheck`, or `pip-audit`).
3. **Save Scan Evidence:**
   - Save combined secret-scan and SCA/SBOM findings output to `docs/tasks/logs/sca-[timestamp].log`.
4. **Block on Leaks or CRITICAL CVEs:**
   - **Block Phase 7 completion** if any leaked secret or `CRITICAL` dependency vulnerability is discovered. Remediate (revoke/rotate/externalize secret, upgrade or replace vulnerable dependency) and re-scan clean before proceeding.

## Output Contract
Scan log saved at `docs/tasks/logs/sca-[timestamp].log` verifying zero leaked secrets and zero unresolved `CRITICAL` dependency vulnerabilities.

## Checklist
- [ ] Full repository secret scan executed via GoThreatScope, ironward, GitHub secret scanning, or CLI fallback
- [ ] SBOM / dependency audit run against OSV.dev via GoThreatScope, `@infoinlet/mcp-codeaudit`, or stack audit CLI
- [ ] Raw log written to `docs/tasks/logs/sca-[timestamp].log`
- [ ] Zero hardcoded secrets and zero unresolved `CRITICAL` dependency vulnerabilities

## Prohibitions
- Leaving committed credentials in code, tests, or git-tracked `.env` files.
- Proceeding past Phase 7 with an unresolved `CRITICAL` CVE or leaked secret.

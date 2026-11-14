---
name: agentic-security-guard
description: Audits agent workflows, tools, permissions, and multi-agent boundaries against the OWASP Top 10 for Agentic Applications 2026 (ASI01-ASI10) in Phase 7 and logs compliance under Agentic Security in docs/tasks/AUDIT.md.
version: 5.0.0
status: active
---

# Agentic Security Guard

## Trigger
Phase 7 (Audit).

## Process
1. Inspect agent definitions, workflow hooks, MCP tool bindings, shell/CLI wrappers, credential scopes, and any built application agent features against the **OWASP Top 10 for Agentic Applications 2026 (`ASI01`-`ASI10`)**.
2. Focus specifically on critical agentic threat vectors:
   - **ASI02 (Tool Misuse):** Verify the agent does not expose overly dangerous tools or execute arbitrary shell commands, file writes, or outbound requests without strict parameter validation, allowlists, or `human-in-the-loop` approval.
   - **ASI03 (Identity & Privilege Abuse):** Verify agents do not inherit excessive user/cloud/database privileges or rely on long-lived ambient credentials; enforce short-lived, least-privilege scoped tokens.
   - **ASI05 (Multi-Agent Privilege Escalation):** In multi-agent flows (such as subagents or `/transactional-wave`), verify that delegated agents cannot escalate permissions across workspace, git branch, or tenant boundaries (`boundary-guard`).
   - Also inspect prompt/goal hijack (**ASI01**) and memory poisoning protections (**ASI06**, enforced via `lessons-learned`).
3. Document compliance status, checked controls, and any required fixes in `docs/tasks/AUDIT.md` under **`## Agentic Security`**.
4. Block Phase 8 delivery on any `HIGH` or `CRITICAL` agentic security gap until remediated.

## Output Contract
An **`## Agentic Security`** compliance section recorded in `docs/tasks/AUDIT.md` covering `ASI01`-`ASI10` (explicitly verifying `ASI02`, `ASI03`, and `ASI05`).

## Checklist
- [ ] Audited against OWASP Top 10 for Agentic Applications 2026 (`ASI01`-`ASI10`)
- [ ] `ASI02` (Tool Misuse) shell/tool validation controls verified
- [ ] `ASI03` (Identity & Privilege Abuse) least-privilege scoping verified
- [ ] `ASI05` (Multi-Agent Privilege Escalation) boundary isolation verified
- [ ] Results documented under `## Agentic Security` in `docs/tasks/AUDIT.md`

## Prohibitions
- Exposing unvalidated shell execution, wildcard write access, or root/admin credentials to autonomous or subagent loops.
- Skipping `ASI02`, `ASI03`, or `ASI05` checks when shipping agentic tooling or multi-agent features.

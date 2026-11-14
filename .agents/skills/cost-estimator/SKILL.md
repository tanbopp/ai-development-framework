---
name: cost-estimator
description: Estimates token consumption from project feature count and target platform before /auto-build or Phase 1, warning users on large projects and offering /full-cycle as a step-by-step alternative.
version: 4.0.0
status: active
---

# Cost Estimator

## Trigger
Mandatory before starting `/auto-build` or Phase 1 (Planning), and re-evaluated once `docs/project/PRD.md` defines the feature set.

## Process
1. **Assess Project Complexity:**
   - Determine the expected number of features (`REQ-XXX` groups in `docs/project/PRD.md`, or features described in the user's initial `<idea>` when run prior to Phase 1) and the target platform (`Web`, `Mobile`, `Desktop`, `Plugin`, `CLI`, or multi-platform).
2. **Classify Token Usage Tier:**
   - `Low`: <= 3 small features on a single platform with minimal external integrations.
   - `Medium`: 4 to 5 features on a single platform, or standard DB/auth setup.
   - `High`: > 5 features (>= 15 vertical-slice tasks), multi-platform or Desktop/Mobile builds, or autonomous `/auto-build` end-to-end execution with audit loops.
3. **Output Cost Warning on Medium/High Scope (or Before `/auto-build`):**
   - Present a clear advisory message using this template:
     `"This project has X features and targets [Platform]. Estimated token usage: [High/Medium/Low]. Proceed with /auto-build? Or use /full-cycle to review step-by-step?"`
4. **Informational Safeguard Only:**
   - Keep this check purely informational so users do not unexpectedly drain API limits; under `/auto-build`, pause only for confirmation if estimated usage is `High` (unless the user already confirmed proceeding).

## Output Contract
A concise complexity and token-usage summary (`Low`, `Medium`, or `High`) surfaced before `/auto-build` or Phase 1 execution begins.

## Checklist
- [ ] Feature count and target platform assessed
- [ ] Token usage tier (`Low` / `Medium` / `High`) assigned transparently
- [ ] Large project / `/auto-build` warning displayed offering `/full-cycle` alternative

## Prohibitions
- Hiding heavy autonomous token costs on large multi-feature projects before `/auto-build`.
- Fabricating exact billing dollar amounts or API quota figures not available in the environment.

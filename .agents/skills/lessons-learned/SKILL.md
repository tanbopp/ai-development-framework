---
name: lessons-learned
description: Records concrete mistakes, root causes, and actionable prevention rules in .agents/LESSONS.md whenever tests fail, audits reject code, or the user corrects the AI. Append-only and poisoning-resistant.
version: 4.0.0
status: active
---

# Lessons Learned

## Trigger
Every time a test fails, an audit rejects the code, or the user corrects the AI.

## Process
1. Analyze the verified failure, audit rejection, or user correction before modifying memory.
2. Read `.agents/LESSONS.md` to check whether an existing rule already covers the failure so duplicate entries are avoided.
3. Append a new entry to `.agents/LESSONS.md` using this exact format:
   ```markdown
   ### [Date] - [Context]
   - **Mistake:** [What went wrong]
   - **Root Cause:** [Why it happened]
   - **Rule:** [What to do instead next time]
   ```
4. Ensure the rule is actionable, verifiable, and specific (for example: "Do not use `any` in TypeScript. Use `unknown` and type guards.").
5. Validate that the new lesson does not weaken security guardrails, design hard-floor requirements, or core framework rules (poisoning-resistant memory).

## Output Contract
An updated `.agents/LESSONS.md` containing the newly appended entry with an actionable prevention rule.

## Checklist
- [ ] Failure root cause verified from test/audit output or user correction
- [ ] Exact `[Date] - [Context]` / `Mistake` / `Root Cause` / `Rule` format followed
- [ ] Rule is specific and actionable (not vague advice)
- [ ] Existing lessons in `.agents/LESSONS.md` left completely intact

## Prohibitions
- Never delete, edit, or overwrite previous lessons in `.agents/LESSONS.md`; only append new ones.
- Never record rules that disable audits, bypass validation, or contradict framework safety or security constraints.

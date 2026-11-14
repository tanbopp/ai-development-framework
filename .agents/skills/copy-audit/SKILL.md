---
name: copy-audit
description: Audits text output (landing pages, microcopy, documentation) for AI-writing tells such as negative parallelism, throat-clearing openers, em-dash overuse, and filler vocabulary. Text only, never CSS or components. Use before delivering any output containing text.
version: 4.0.0
status: active
---

# Copy Audit

## Trigger
Before delivering any output containing text (landing page, microcopy, documentation).

## Process
Scan the text for AI-writing tells:
- Negative parallelism ("Not X, but Y")
- Throat-clearing openers ("In today's fast-paced world")
- Excessive em dashes
- Unnecessary AI vocabulary ("delve", "leverage", "seamless")
- Forced rule of three
- Vague attribution ("experts believe")

For each hit, record location, the tell, and a concrete replacement.

## Output Contract
A report of text to fix, with replacement examples.

## Checklist
- [ ] Every text file in scope scanned
- [ ] Each finding has a replacement

## Prohibitions
- Auditing CSS or components.
- Rewriting text that is a required format (legal text, API names, quoted sources).


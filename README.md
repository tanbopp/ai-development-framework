# AI Development Framework

A reusable, markdown-based framework for AI-assisted development that is spec-driven, stack-agnostic, and built to resist "AI slop".

## 1. What this is

A template of rules, agents, skills, and workflows. You supply one application idea. The agent walks a gated pipeline (plan, stack, design, spec, tasks, implement, audit, deliver) and writes every handover as a markdown file you can commit and review.

## 2. Who it is for

Developers who want to build applications with AI without losing control over stack, design, and quality.

## 3. How to install

1. Use this repository as a GitHub template, or clone it into a new workspace.
2. Run `/init-project`. It fetches the vendor repos and lists what is missing.
3. Run `/full-cycle <idea>`.

Manual equivalents of `/init-project`:

```bash
git clone --depth 1 https://github.com/DietrichGebert/ponytail.git .agents/vendor/ponytail
git clone --depth 1 https://github.com/changemaner/design-taste-frontend.git .agents/vendor/design-taste-frontend
npx skills add nutlope/hallmark
npx -y unslop-mcp@latest init
```

Windows users: Antigravity runs commands in PowerShell, not bash. The `/init-project` workflow handles this automatically. If you want to run the manual commands yourself, use the PowerShell block in `.agents/workflows/init-project.md` instead of the bash block.

`/init-project` handles the `.gitkeep` placeholders automatically. If you prefer to run the clone commands manually, run them after deleting the corresponding `.gitkeep` file.

**The vendor directories are intentionally empty in this template.** `.agents/vendor/*` contains only `.gitkeep` files, because the vendor repos keep their own licenses and are fetched at install time. You must run `/init-project` before `/full-cycle`. Without it, Ponytail and design-taste-frontend are missing, and Hallmark and unslop are not installed, so the design audits will not function. Phases 1 to 5 can still produce documents, but the Phase 7 audit cannot run its tools.

Hallmark can also be cloned manually into `.agents/vendor/hallmark/`. `unslop init` writes agent config files (for example `.mcp.json`) and merges into existing ones, so review the diff before committing.

## 4. Architecture

```
.agents/
  rules/       Always-on and contextual rules (YAML frontmatter with a valid trigger is required)
  agents/      planner, architect, frontend, qa, devops
  skills/      prd-generator, stack-advisor, design-system-generator, tech-spec-writer,
               task-breakdown, ponytail-wrapper, design-audit, copy-audit
  workflows/   init-project, plan-feature, full-cycle
  vendor/      ponytail, hallmark, design-taste-frontend, unslop (fetched by /init-project)
docs/
  project/     PRD, STACK, DESIGN, ARCHITECTURE, DECISIONS (see the .example files)
  specs/       Per-feature specs, each under 250 lines
  tasks/       TASKS.md and audit or deploy reports
.github/workflows/pr-quality.yaml   Pull-request quality gate
AGENTS.md      Always-on pointers for the agent
```

Rules without valid frontmatter are silently discarded by Antigravity, and rule files are truncated at 24,000 bytes. Every rule here stays far below that.

## 5. Integrated repos and honest trade-offs

**Hallmark** ([Nutlope/hallmark](https://github.com/Nutlope/hallmark)). A design skill with a lint script and a browser render check. Its gate table has 87 rows covering 70 distinct gate numbers: 39 Floor rows and 48 Reflex rows. Finish is a separate tier of craft checks reviewed outside that table. Floor never ships, Reflex may be overruled on the record with a reason, and Finish never blocks. It has three verbs: build, redesign, and audit. In this framework it is used as an auditor only. Trade-off: it carries strong house opinions (for example, no eyebrow labels), and the render checks need Node, `puppeteer-core`, and Chrome. Without them only the static checks run.

**design-taste-frontend** ([changemaner/design-taste-frontend](https://github.com/changemaner/design-taste-frontend)). 30 brand design libraries, each with `DESIGN.md`, `tokens.json`, `variables.css`, and `theme.css`, plus four machine QA gates. It is the only design generator here. Trade-off: the documentation is mostly in Chinese, and the brand assets carry a separate license position (see Licenses and notices).

**unslop** ([avivgerz-hue/unslop](https://github.com/avivgerz-hue/unslop)). A CLI and MCP server with 72 rules across seven pillars and a Design Craft Score from 0 to 100. Trade-off: a score is not a verdict. Several rules are subjective, static analysis does not replace rendered checks, and `init` modifies agent config files. Audits here allow overrides with written justification in `DESIGN.md`.

**peakoss/anti-slop** ([peakoss/anti-slop](https://github.com/peakoss/anti-slop)). A GitHub Action with 34 behavioral and structural PR checks (account age, fork velocity, merge history, template compliance, and similar). It is not an AI-text detector. Trade-off: untuned, it can flag legitimate first-time contributors, and its default action closes the PR. See the next section for how this template configures it.

**Ponytail** ([DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)). A structured YAGNI guardrail: a short rule that makes the agent check whether code needs to exist before writing it. The shipped rule file has 6 numbered rungs. The README ladder image shows 7. This framework's wrapper uses the 7-rung version. Modes are `lite`, `full`, and `ultra`; use `lite` or turn it off for trivial tasks. The repository contains no third-party audit. Its published benchmark figures come from the author's own setup, so treat them as one data point, not a guarantee. Trade-off: it can make the agent under-build if you let it, so keep the tests and audits.

**Spec Kit** ([github/spec-kit](https://github.com/github/spec-kit)) and **OpenSpec** ([Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)). Neither is installed. This framework is inspired by both: constitution, specify, plan, tasks, implement, converge from Spec Kit, and the propose, explore, apply, archive cycle from OpenSpec. The `full-cycle` workflow adopts the ideas, not the tools. Trade-off: you do not get their CLIs or their change-archive tooling.

## 6. Pull-request quality gate

[`.github/workflows/pr-quality.yaml`](.github/workflows/pr-quality.yaml) runs `peakoss/anti-slop@v0` with `max-failures: 6` and `close-pr: false`.

This is an observe-only setup. Upstream defaults to `max-failures: 4` and `close-pr: true`. Closing a PR automatically is a high-cost action for a public template, because a false positive turns away a real contributor with no human in the loop. With `close-pr: false`, a failing PR gets a label (`needs-review`) and a comment, and a maintainer decides. Upstream has no `dry-run` input, so this is the closest equivalent. See the ADR example in `docs/project/DECISIONS.md.example`.

Once you have watched the gate on real traffic and tuned the thresholds, you can lower `max-failures` or set `close-pr: true`. Record that change as an ADR.

## 7. Workflow

Plan, Stack, Design, Spec, Task, Implement, Audit, Deliver, with a gate at every planning phase.

| Phase | Output | Gate |
| --- | --- | --- |
| Plan | `PRD.md` | Gate 1 |
| Stack | `STACK.md` and an ADR | Gate 2 |
| Design | `DESIGN.md` and tokens | Gate 3 |
| Spec | `ARCHITECTURE.md` and `docs/specs/` | Gate 4 |
| Task, Implement | `TASKS.md`, code | none |
| Audit | audit report | hard floor must pass |
| Deliver | build and deployment report | user review |

Typing 'continue', 'proceed', or 'lanjut' counts as approval of the current gate. To reject a spec and request changes, say so explicitly.

Audit has two tiers. The hard floor (WCAG AA contrast, focus state, `prefers-reduced-motion`, mobile overflow or truncated text) always fails the audit. Soft preferences may be overridden with a written brand justification in `DESIGN.md`.

## 8. Philosophy

Stack and design are decided per project. What stays fixed is process, gates, and the audit trail. Every spec file stays under 250 lines because long context lowers instruction adherence.

## 9. Honest expectations

This is not a one-click app tool. It is a development process that needs your review at every gate. If you want to approve decisions along the way, it will help. If you expect a finished app from one prompt, it will disappoint you.

## 10. Customization

- Stack principles: edit `.agents/rules/01-stack-principles.md`.
- Design principles and the hard floor: edit `.agents/rules/02-design-principles.md`.
- Pipeline and gates: edit `.agents/workflows/full-cycle.md`.
- Keep the frontmatter `trigger` on every rule, or it is discarded.
- Skipping a gate is allowed only as an accepted risk recorded in `docs/project/DECISIONS.md`.

## 11. Licenses and notices

This repository is MIT licensed (see [LICENSE](LICENSE)). That license covers the framework files only. Vendor repositories fetched into `.agents/vendor/` keep their own licenses.

**design-taste-frontend brand assets are style reference only and are not covered by MIT.** The 30 brand design libraries (`refero-styles/`) and its brand style report are described by that project as style reference, not affiliated with the brands, with brand names, trademarks, and assets belonging to their owners. If you ship a design that copies a specific brand's look, confirm usage with the brand owner. This template does not redistribute those assets. They are fetched by `/init-project` at your request, and you are responsible for how you use them. Hallmark's photo library lists Wikimedia Commons images that each keep their own license and need credit on the page.

See also [LICENSE-NOTICE.md](LICENSE-NOTICE.md).


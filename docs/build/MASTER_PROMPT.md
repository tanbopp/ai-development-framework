# MASTER PROMPT — Build a Reusable, Publishable "AI Development Framework" Template

You are a Principal AI Systems Architect. Your task is to build a **reusable, markdown-based development framework** in this empty workspace. This framework will be published as an open-source GitHub template. Anyone who installs it must be able to provide a single application idea, and the AI agent will automatically produce the entire planning, specification, task, implementation, and audit pipeline — without "AI slop", with a consciously chosen stack and design system per project.

Use Claude Opus with high reasoning. Read this entire instruction before writing the first file. Do not stop midway. Do not ask for confirmation during execution. Complete Steps 0 through 12, then stop at Step 13 (Validation) for my review.

**CRITICAL LANGUAGE RULE:** Every file you create — rules, agents, skills, workflows, README, examples, comments, frontmatter descriptions, commit-style notes — MUST be written in **English only**. Do not write any prose in Indonesian, regardless of the language I used to write this prompt. Filenames, directory names, section headings, and example content must all be English.

---

## CORE PRINCIPLES — NON-NEGOTIABLE

1. **Stack is NOT globally locked.** The framework provides universal principles only. The concrete stack is decided per project by the Architect Agent based on the PRD, then approved by the user before implementation.
2. **Design system is NOT globally locked.** Concrete design tokens are generated per project. Only design principles and anti-patterns are universal.
3. **What is static is the "soul":** process, gates, quality principles, audit trail. What is dynamic is the technical embodiment.
4. **Every artifact is a markdown file.** Handover between phases happens through files, not conversation. This creates a committable audit trail.
5. **Gates must not be skipped.** If the user asks to skip, record it as an accepted risk in `DECISIONS.md`.
6. **Verbosity is the enemy.** Context bloat reduces instruction adherence. Every file must be concise; every spec must be reviewable in under 10 minutes.
7. **Design audit is tiered.** Distinguish hard floor (must fail) from soft preference (may override with written brand justification).

---

## STEP 0 — RESEARCH AND VERIFICATION (REQUIRED BEFORE WRITING ANY FILE)

Research the following repos and documentation. Record the trigger, output, and installation method for each. Do not modify the original repo contents; adapt only at the wrapper layer.

### 0.1 Antigravity documentation
- Read Antigravity rules documentation: every file in `.agents/rules/*.md` **must** have YAML frontmatter with a valid `trigger` (`always_on`, `model_decision`, `glob`, `manual`). Without valid frontmatter, the rule is **silently discarded**.
- Read Agent Skills documentation: a skill is a directory at `<workspace-root>/.agents/skills/<skill-folder>/SKILL.md`. Minimum frontmatter: `name` (max 64 characters) and `description` (max 1,024 characters). The body is loaded progressively — only metadata is always in context.
- Note: `AGENTS.md` and `GEMINI.md` require no frontmatter; their entire content is treated as `always_on`.

### 0.2 Anti-slop and design repos
- `Nutlope/hallmark` — read the README. Note: 57 slop-test gates across **Floor** (must not pass: contrast, focus, reduced-motion, fabricated facts, broken mobile layout), **Reflex** (model default; may override with recorded reason), and **Finish** (craft details that do not block ship). Four verbs: build, audit, redesign, study. Install via `npx skills add nutlope/hallmark`.
- `changemaner/design-taste-frontend` — read the repo structure. Note: 30 brand libraries, each containing `DESIGN.md`, `tokens.json`, `variables.css`, `theme.css`, and 4 machine QA gates. Clone into `.agents/vendor/design-taste-frontend/`.
- `avivgerz-hue/unslop` — read the README. Note: 72 rules across 7 design pillars, producing a Design Craft Score from 0–100, CLI + MCP server. Install via `npx -y unslop-mcp@latest init`. **Important:** not every rule is universal; several are subjective preferences. The audit must allow overrides with justification in `DESIGN.md`.
- `peakoss/anti-slop` — read the README and the unslop research on this action. Note: 34 behavioral and structural checks (account age, merge ratio, fork velocity, PR template compliance, emoji/code-reference density) — **not** an AI-word detector. Default `max-failures: 4`; can false-positive on legitimate first-time contributors if untuned. The default template must be more permissive.

### 0.3 Anti-overengineering repo
- `DietrichGebert/ponytail` — read the README and the independent audit. Note: Ponytail is essentially **YAGNI in ~100 lines of markdown** with a 7-step decision ladder. The original benchmark claimed 54–94% code reduction, but the independent audit found that the three words "Follow YAGNI principles" nearly matched its score, and seven words beat it. Treat Ponytail as a **structured YAGNI guardrail**, not a magic solution. Provide three modes: `lite`, `full`, `ultra`; for trivial tasks use `lite` or disable it.

### 0.4 SDD references
- `github/spec-kit` — note the flow: constitution → specify → plan → tasks → implement → converge.
- `Fission-AI/OpenSpec` — note the flow: propose → explore → apply → archive, with an optional Analyze step to check spec-plan-task consistency before coding.
- **Important from SDD research:** context bloat reduces instruction compliance. Target every spec file at under 200–250 lines. Split specs per feature in `docs/specs/`; do not pile everything into one file. The constitution must be dense, not an omnibus.

### 0.5 Technical limits that must not be ignored
- Any `rules/*.md` without valid frontmatter → silently discarded.
- Any `rules/*.md` is truncated at 24,000 bytes.
- Overly long spec files reduce adherence and slow down review gates.
- An untuned `peakoss/anti-slop` can hit legitimate contributors.

---

## STEP 1 — CREATE THE DIRECTORY STRUCTURE

Create the following structure at the workspace root. Every directory must exist, even if some files will only be filled in later steps.

```
.
├── .agents/
│   ├── rules/
│   │   ├── 00-constitution.md
│   │   ├── 01-stack-principles.md
│   │   ├── 02-design-principles.md
│   │   ├── 03-precedence.md
│   │   └── 04-stack-selection-process.md
│   ├── agents/
│   │   ├── planner/agent.md
│   │   ├── architect/agent.md
│   │   ├── frontend/agent.md
│   │   ├── qa/agent.md
│   │   └── devops/agent.md
│   ├── skills/
│   │   ├── prd-generator/SKILL.md
│   │   ├── stack-advisor/SKILL.md
│   │   ├── design-system-generator/SKILL.md
│   │   ├── tech-spec-writer/SKILL.md
│   │   ├── task-breakdown/SKILL.md
│   │   ├── ponytail-wrapper/SKILL.md
│   │   ├── design-audit/SKILL.md
│   │   └── copy-audit/SKILL.md
│   ├── workflows/
│   │   ├── init-project.md
│   │   ├── plan-feature.md
│   │   └── full-cycle.md
│   └── vendor/
│       ├── ponytail/
│       ├── hallmark/
│       ├── design-taste-frontend/
│       └── unslop/
├── docs/
│   ├── project/
│   │   ├── .gitkeep
│   │   ├── PRD.md.example
│   │   ├── STACK.md.example
│   │   ├── DESIGN.md.example
│   │   ├── ARCHITECTURE.md.example
│   │   └── DECISIONS.md.example
│   ├── specs/
│   │   └── .gitkeep
│   └── tasks/
│       └── .gitkeep
├── .github/
│   └── workflows/
│       └── pr-quality.yaml
├── AGENTS.md
├── README.md
├── LICENSE
└── .gitignore
```

---

## STEP 2 — WRITE THE RULES (VALID FRONTMATTER REQUIRED)

Every file in `.agents/rules/` **must** begin with YAML frontmatter. Without it, the rule is silently discarded. Use `trigger: always_on` for the constitution and precedence; use `trigger: model_decision` for contextual rules (stack-principles, design-principles, stack-selection-process) with a clear `description` so the agent loads them when relevant.

### 2.1 `00-constitution.md`

Frontmatter: `trigger: always_on`. Content max 500–800 tokens. Write as a concise list of non-negotiable principles:

1. Spec is the source of truth. Code follows the spec, not the reverse.
2. If something is ambiguous, the agent MUST stop and ask. Guessing is forbidden.
3. Every requirement has a unique ID (`REQ-XXX`) and binary acceptance criteria (yes/no).
4. Out-of-scope must be explicit in every PRD.
5. Every output must pass audit before delivery.
6. The stack is decided per project, not locked upfront.
7. The design system is generated per project, not locked upfront.
8. Every major technical decision is recorded as an ADR in `DECISIONS.md`.
9. Adding a new dependency without user approval is forbidden.
10. Verbosity is the enemy: every spec file must be under 250 lines; every review gate must be completable in under 10 minutes.

### 2.2 `01-stack-principles.md`

Frontmatter: `trigger: model_decision`; `description: "Apply when selecting, validating, or reviewing the technology stack for a project. Enforces universal engineering quality principles without locking concrete frameworks."`. Content — universal principles, not a stack lock:

1. Statically typed languages for application code (TypeScript, Go, Rust, Kotlin, etc.). If a dynamic language is chosen, written justification is required in `STACK.md`.
2. Strict mode is on (TS strict, Go vet, Rust clippy).
3. Testing is mandatory: unit tests for business logic, E2E tests for critical flows.
4. No `any` (or similar escape hatch) without a justification comment.
5. No new dependency without approval; every dependency is recorded in `STACK.md` with its rationale.
6. No commented-out dead code.
7. No secrets in code. Use environment variables.
8. Every stack decision must reference at least two requirements from the PRD.

### 2.3 `02-design-principles.md`

Frontmatter: `trigger: model_decision`; `description: "Apply when generating, reviewing, or auditing UI design. Enforces universal design quality floors and neutral anti-patterns without locking specific tokens or brand aesthetics."`. Content:

1. Design tokens must be defined before implementation, stored in `DESIGN.md`, and exported to the format the stack requires.
2. Minimum three-level typography hierarchy (size or weight).
3. Every component has states: default, hover, focus, disabled, loading, error.
4. WCAG AA contrast minimum of 4.5:1 for normal text.
5. Motion respects `prefers-reduced-motion`.
6. The design system is generated per project; do not reuse another project's tokens just because it is easier.
7. Universal anti-patterns (soft preference, may override with brand justification in `DESIGN.md`): default purple/indigo/violet accent without brand reason; purple-to-blue gradient as default hero; Inter/Roboto/Arial as the sole identity; identical three-card grid with lucide icons in rounded squares; glassmorphism on every surface; `hover:scale-105` on every card.
8. Hard floor that cannot be overridden: WCAG contrast, missing focus state, ignored `prefers-reduced-motion`, text truncated on mobile.

### 2.4 `03-precedence.md`

Frontmatter: `trigger: always_on`. Define the hierarchy so skills do not collide:

1. Design authority: `design-taste-frontend` is the only design generator.
2. Design audit: Hallmark + unslop only audit, never generate new design.
3. Copy audit: text only, not CSS or components.
4. Code minimalism: Ponytail applies only during implementation, not design.
5. CI gate: `peakoss/anti-slop` applies only to PR gates.
6. Project stack and design tokens ALWAYS win over the agent's default preferences.
7. If two rules conflict, the rule with the lower number wins.

### 2.5 `04-stack-selection-process.md`

Frontmatter: `trigger: model_decision`; `description: "Apply when the Architect Agent needs to propose a technology stack for a new project based on approved PRD requirements."`. Content:

1. The Planner produces a PRD with a "Non-Functional Requirements" section (user scale, realtime, offline, mobile, AI/LLM, third-party integrations, deployment budget, maintaining team).
2. The Architect runs the `stack-advisor` skill: draft 2–3 stack options, each with trade-offs, a recommendation referencing at least two `REQ-XXX`, and a "What We Deliberately Do Not Use" list with reasons.
3. The Architect writes `STACK.md` and an ADR in `DECISIONS.md`.
4. The Architect STOPS. The user approves or changes. If changed, return to step 2.
5. After approval, proceed to `design-system-generator`, then `tech-spec-writer`.
Prohibitions: choosing a stack before the PRD exists; choosing a stack without alternatives; using a default stack only because it is "familiar".

---

## STEP 3 — WRITE THE AGENTS

Each agent is a markdown file at `.agents/agents/<name>/agent.md`. Every agent must have: **Role**, **Responsibilities**, **Output Contract**, **Prohibitions**, and **Handover** (which files it reads and writes).

### 3.1 `planner/agent.md`
Role: Technical Product Manager. Turns a raw idea into an executable PRD, then breaks it into tasks.
Responsibilities: runs `prd-generator` and `task-breakdown`. Does not write code. Must include a "Non-Functional Requirements" section in the PRD as input for `stack-advisor`.
Output Contract: `docs/project/PRD.md` and `docs/tasks/TASKS.md`. Every requirement has a unique ID and binary acceptance criteria. Out-of-scope is explicit.
Prohibitions: filling gaps with assumptions; skipping approval gates; writing technical specs.

### 3.2 `architect/agent.md`
Role: Technical architect. Runs `stack-advisor`, `design-system-generator`, and `tech-spec-writer`.
Responsibilities: proposes a stack based on the PRD; generates design tokens; writes architecture and per-feature specs. Stops at every gate for user approval.
Output Contract: `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, and files in `docs/specs/`. Every major technical decision is recorded as an ADR.
Prohibitions: choosing a stack before the PRD is approved; writing technical specs before `STACK.md` and `DESIGN.md` are approved; deviating from approved tokens without updating `DESIGN.md`.

### 3.3 `frontend/agent.md`
Role: Frontend implementer. Translates specs and design tokens into code.
Responsibilities: implements tasks one by one; must use `ponytail-wrapper` to prevent overengineering; must reference `DESIGN.md` for every visual decision.
Output Contract: code that passes `design-audit` and `copy-audit`; tasks marked complete in `TASKS.md`.
Prohibitions: adding colors/fonts outside the tokens; adding dependencies without approval; bypassing audits.

### 3.4 `qa/agent.md`
Role: Auditor. Runs `design-audit`, `copy-audit`, and verifies test coverage.
Responsibilities: rejects output that violates the hard floor; for soft preferences, checks the justification in `DESIGN.md` before rejecting. Writes a specific fix list if failed.
Output Contract: audit report with pass/fail status per item; an actionable fix list.
Prohibitions: going soft; generating new design; ignoring the hard floor.

### 3.5 `devops/agent.md`
Role: Delivery. Build, deployment validation, and CI gate.
Responsibilities: runs the build; validates the `peakoss/anti-slop` workflow; ensures deployment succeeds.
Output Contract: build artifact that passes; deployment report.
Prohibitions: modifying the CI gate configuration without maintainer approval.

---

## STEP 4 — WRITE THE SKILLS (STANDARD SKILL.md FORMAT)

Each skill is a directory containing `SKILL.md`. Minimum frontmatter: `name` (max 64 characters) and `description` (max 1,024 characters). The body must be executable: trigger, numbered process, output contract, checklist, and prohibitions. Keep the body concise; use progressive disclosure — put long details in a separate reference file inside the skill directory if needed.

### 4.1 `prd-generator/SKILL.md`
Trigger: The Planner needs to produce a PRD from an idea.
Process:
1. Read `AGENTS.md`, `00-constitution.md`, and `04-stack-selection-process.md`.
2. From the user's idea, extract Goal, Users, Out of Scope, Requirements, and **Non-Functional Requirements** (scale, realtime, offline, mobile, AI, integrations, budget, maintaining team).
3. Each requirement: ID `REQ-XXX`, verifiable statement, priority (must/should/could), rationale, and binary acceptance criteria.
4. Write `docs/project/PRD.md`. Keep it under 250 lines. If longer, split per feature into `docs/specs/PRD-<feature>.md` and make `PRD.md` the index.
5. Stop and request user approval.
Output Contract: PRD with 12 sections: Goal, Users, Out of Scope, Requirements, Non-Functional Requirements, Decided-Do-Not-Change, Open Questions, Agent Instructions, Success Metrics, Risks, Dependencies, Glossary.
Prohibitions: guessing ambiguous requirements; skipping the Non-Functional Requirements section.

### 4.2 `stack-advisor/SKILL.md`
Trigger: The Architect after the PRD is approved.
Process:
1. Read the PRD and `01-stack-principles.md`.
2. Extract non-functional requirements.
3. Draft 2–3 stack options. Each option contains: language + framework, database/storage, hosting/deployment, testing, trade-offs (what is won and what is sacrificed), and fit with the requirements.
4. Provide a recommendation referencing at least two `REQ-XXX`.
5. Write `docs/project/STACK.md` and a short ADR in `DECISIONS.md`.
6. Stop and request user approval.
Output Contract: `STACK.md` with sections Non-Functional Requirements, Option A/B/C, Recommendation, Rationale, What We Deliberately Do Not Use, and Open Questions.
Prohibitions: choosing a stack before the PRD exists; choosing without alternatives; using a default stack without checking the PRD.

### 4.3 `design-system-generator/SKILL.md`
Trigger: After `STACK.md` is approved.
Process:
1. Read the PRD, `STACK.md`, and `02-design-principles.md`.
2. Identify the tone/brand from the PRD (formal, playful, clinical, editorial, etc.).
3. Choose or create consistent tokens: color palette, typography (font, scale, weight hierarchy), spacing, radius, shadow, motion.
4. Export tokens to the format matching the stack (CSS variables, Tailwind config, Style Dictionary, etc.).
5. If adopting a brand library from `design-taste-frontend`, choose exactly ONE library that fits best and record the reason in `DESIGN.md`.
6. Write `docs/project/DESIGN.md` and the token files.
7. Stop and request user approval.
Output Contract: `DESIGN.md` with full tokens, hard floor, soft preferences, and brand justification for any override.
Prohibitions: reusing another project's tokens just because it is easier; adding colors/fonts after approval without updating `DESIGN.md`.

### 4.4 `tech-spec-writer/SKILL.md`
Trigger: After `DESIGN.md` is approved.
Process:
1. Read `PRD.md`, `STACK.md`, and `DESIGN.md`.
2. Write `ARCHITECTURE.md`: components, data flow, boundaries, error handling, observability, testing strategy.
3. Write per-feature specs in `docs/specs/<feature>.md`. Each spec is at most 250 lines; each spec references `REQ-XXX`.
4. Stop and request user approval.
Output Contract: `ARCHITECTURE.md` + per-feature spec files that can be reviewed in under 10 minutes each.
Prohibitions: deviating from the approved stack or tokens; writing specs longer than the limit without splitting them.

### 4.5 `task-breakdown/SKILL.md`
Trigger: After the technical specs are approved.
Process: break the spec into small, verifiable tasks. Each task: ID, description, files touched, acceptance criteria, dependencies, estimate.
Output Contract: `docs/tasks/TASKS.md` with a checklist that can be ticked off.
Prohibitions: tasks too large to review; tasks without acceptance criteria.

### 4.6 `ponytail-wrapper/SKILL.md`
Trigger: Every implementation task (frontend/backend).
Process:
1. Run the 7-step decision ladder: (1) does it need to exist? (2) does it already exist in the codebase? (3) can stdlib do it? (4) can platform-native do it? (5) can an installed dependency do it? (6) one line? (7) only then write minimal code.
2. Use `lite` mode for trivial tasks, `full` for medium-to-large tasks, `ultra` only for refactors / code reduction.
3. Must preserve security guardrails: validate trust boundaries; do not sacrifice security for brevity.
Output Contract: minimal code that passes tests and audits.
Prohibitions: sacrificing security; using `ultra` mode on unstable new features.

**Honest documentation in this skill:** Ponytail is essentially structured YAGNI. It is effective at preventing overengineering on medium-to-large tasks. For trivial tasks, use `lite` mode or disable it. Do not promise 54–94% code reduction as a universal guarantee; those are benchmark numbers from specific contexts.

### 4.7 `design-audit/SKILL.md`
Trigger: Before delivering frontend output.
Process:
1. Run hard-floor checks (must fail if violated): WCAG AA contrast, focus state, `prefers-reduced-motion`, mobile overflow, truncated text.
2. Run soft-preference checks (Hallmark Reflex/Finish + unslop): if violated, check `DESIGN.md` — is there written brand justification? If yes, skip. If no, request a fix.
3. Produce a per-item report: pass/fail, tier (hard/soft), rationale, and suggested fix.
Output Contract: audit report; reject if any hard-floor violation exists; for soft preferences, reject only when there is no justification.
Prohibitions: imposing a single design taste on every project; going soft on the hard floor.

### 4.8 `copy-audit/SKILL.md`
Trigger: Before delivering any output containing text (landing page, microcopy, documentation).
Process: detect AI-writing tells: negative parallelism, throat-clearing opener, excessive em dashes, unnecessary AI vocabulary ("delve", "leverage", "seamless"), forced rule of three, vague attribution ("experts believe").
Output Contract: report of text that needs fixing, with replacement examples.
Prohibitions: auditing CSS or components; rewriting text that is a required format.

---

## STEP 5 — WRITE THE WORKFLOWS

Workflows are slash commands that bind agents and skills into a pipeline. Even though Antigravity has migrated to Agent Skills, the `workflows/` directory still serves as pipeline documentation and can be invoked as a composite skill. Write each workflow as clear markdown.

### 5.1 `init-project.md`
Trigger: `/init-project`
Process: ensure rules/agents/skills/workflows exist; clone required vendor repos into `.agents/vendor/`; run `npx skills add nutlope/hallmark` and `npx -y unslop-mcp@latest init` if not already installed; display a readiness checklist.

### 5.2 `plan-feature.md`
Trigger: `/plan-feature <idea>`
Process: Planner → `prd-generator` → `docs/project/PRD.md`. **GATE 1:** stop and request user approval. Do not proceed to stack selection before the PRD is approved.

### 5.3 `full-cycle.md`
Trigger: `/full-cycle <idea>`
Process with sequential gates:
- Phase 1 — Planning: Planner → `prd-generator` → `PRD.md`. **GATE 1:** stop, user reviews PRD.
- Phase 2 — Stack Decision: Architect → `stack-advisor` → `STACK.md` + ADR. **GATE 2:** stop, user reviews and approves the stack.
- Phase 3 — Design System: Architect → `design-system-generator` → `DESIGN.md` + tokens. **GATE 3:** stop, user reviews design tokens.
- Phase 4 — Technical Spec: Architect → `tech-spec-writer` → `ARCHITECTURE.md` + `docs/specs/`. **GATE 4:** stop, user reviews technical specs.
- Phase 5 — Task Breakdown: Planner → `task-breakdown` → `TASKS.md`.
- Phase 6 — Implementation: Frontend/Backend → implement per task with `ponytail-wrapper`.
- Phase 7 — Audit: QA → `design-audit` + `copy-audit` + test coverage. If failing, return to Phase 6 with a specific fix list.
- Phase 8 — Delivery: DevOps → build, `peakoss/anti-slop` CI gate, deployment validation.
Handover Rules: handover between phases must go through markdown files. Gates must not be skipped. If the user asks to skip, record it in `DECISIONS.md` as an accepted risk.

---

## STEP 6 — INTEGRATE THE REAL REPOS

Write the exact installation commands in `init-project.md` and `README.md`.

### 6.1 Ponytail
`git clone --depth 1 https://github.com/DietrichGebert/ponytail.git .agents/vendor/ponytail`

### 6.2 Hallmark
`npx skills add nutlope/hallmark` or manual clone into `.agents/vendor/hallmark/`. Note: Hallmark has Floor (must pass), Reflex (may override with a recorded reason), and Finish (does not block ship) tiers.

### 6.3 design-taste-frontend
`git clone --depth 1 https://github.com/changemaner/design-taste-frontend.git .agents/vendor/design-taste-frontend`

### 6.4 unslop
`npx -y unslop-mcp@latest init`. Note: 72 rules, several of which are subjective; the audit must allow overrides with justification in `DESIGN.md`.

### 6.5 peakoss/anti-slop
Create `.github/workflows/pr-quality.yaml` with a configuration **safe for public release**:

```yaml
name: PR Quality Gate
permissions:
  contents: read
  issues: read
  pull-requests: write

on:
  pull_request_target:
    types: [opened, reopened]

jobs:
  anti-slop:
    runs-on: ubuntu-latest
    steps:
      - uses: peakoss/anti-slop@v0
        with:
          max-failures: 6
          dry-run: true
```

Note in the README: `dry-run: true` and `max-failures: 6` are the defaults for a public template to reduce false positives against legitimate first-time contributors. Maintainers can raise the threshold or enable enforcement mode once confident. This action is behavioral, not an AI-word detector.

### 6.6 Spec Kit and OpenSpec
They do not need to be installed as dependencies. Adopt their philosophy in the `full-cycle` workflow: Specify → Plan → Tasks → Implement → Converge. Add a note in the README that this framework is inspired by both.

---

## STEP 7 — CREATE `AGENTS.md` (MAX 30 LINES)

`AGENTS.md` at the workspace root. No frontmatter (its entirety is `always_on`). Keep it concise:

- **Stack:** decided per project in `docs/project/STACK.md`. Do not assume.
- **Design system:** decided per project in `docs/project/DESIGN.md`. Do not reuse another project's tokens.
- **Entry points:** `docs/project/PRD.md`, `STACK.md`, `DESIGN.md`, `ARCHITECTURE.md`, `docs/tasks/TASKS.md`.
- **Non-negotiable:** read `00-constitution.md` before starting; if ambiguous, stop and ask; audit before delivery.
- **Forbidden:** `any` without justification; colors/fonts outside tokens; new dependencies without approval.
- **Workflow:** use `/full-cycle <idea>` to start a new project.

---

## STEP 8 — CREATE `README.md` (FOR GITHUB)

The README must present this framework honestly and comprehensively. Structure:

1. **What this is:** A reusable framework for AI-assisted development that is anti-slop, spec-driven, and stack-agnostic.
2. **Who it is for:** Developers who want to build applications with AI without losing control over stack, design, and quality.
3. **How to install:** Clone the template into a new workspace, run `/init-project`, then `/full-cycle <idea>`.
4. **Architecture:** Directory tree of `rules/agents/skills/workflows/vendor` with brief explanations.
5. **Integrated repos:** Hallmark, design-taste-frontend, unslop, peakoss/anti-slop, Ponytail, spec-kit, OpenSpec. Include one honest paragraph about the trade-offs of each.
6. **Workflow:** Plan → Stack → Design → Spec → Task → Implement → Audit → Deliver, with a gate at every phase.
7. **Philosophy:** Stack and design are decided per project; what is static is the soul (process, gates, audit). Verbosity is controlled; context bloat is prevented.
8. **Honest expectations:** This is not a "one-click app" tool. It is a development operating system that requires review at every gate. Users who understand this will benefit; users expecting magic will be disappointed.
9. **Customization:** How to change stack principles, design principles, and workflows.
10. **License:** MIT.

---

## STEP 9 — CREATE EXAMPLE FILES IN `docs/project/`

Create a `.example` file for every project artifact. These help users understand the expected format. The example files must be concise and demonstrate structure, not complete content.

- `PRD.md.example`: show all 12 sections with one `REQ-001` as an example.
- `STACK.md.example`: show three stack options for a UMKM inventory app, with a recommendation and trade-offs.
- `DESIGN.md.example`: show minimal tokens (colors, typography, spacing) and one soft-preference override with justification.
- `ARCHITECTURE.md.example`: show components, data flow, and testing strategy concisely.
- `DECISIONS.md.example`: show one ADR for a stack decision.

---

## STEP 10 — CREATE `.gitignore` AND `LICENSE`

`.gitignore`: at minimum `node_modules/`, `.env`, `dist/`, `.DS_Store`, and common build artifacts. Do not ignore `docs/`.

`LICENSE`: MIT.

---

## STEP 11 — VERIFY FRONTMATTER VALIDITY

After all rule files are written, verify that every file in `.agents/rules/*.md` has YAML frontmatter with a valid `trigger`. Display the list of files and their triggers in the validation report. If any are invalid, fix them before proceeding.

---

## STEP 12 — VERIFY VERBOSITY IS UNDER CONTROL

Verify that:
- Every `rules/*.md` file is under 24,000 bytes.
- Every example spec file is under 250 lines.
- `00-constitution.md` is under 800 tokens.
- `AGENTS.md` is under 30 lines.
If anything exceeds the limit, trim or split it. Report the results.

---

## STEP 13 — FINAL VALIDATION (STOP HERE)

After all steps are complete, display:

1. Complete directory tree.
2. Contents of `AGENTS.md` (max 30 lines).
3. List of all `rules/*.md` files with their `trigger` and byte size.
4. A short example PRD output for the idea "UMKM inventory management app" — one page is enough; show `REQ-001` and the Non-Functional Requirements section.
5. An example `STACK.md` output produced by `stack-advisor` for that idea — show three options and a recommendation.
6. Explanation of the audit sequence that will run, including the difference between hard floor and soft preference.
7. Confirmation that `peakoss/anti-slop` is configured with `dry-run: true` and `max-failures: 6`.
8. Confirmation that Ponytail is documented as a YAGNI guardrail, not a magic solution.
9. A statement that the framework is ready to be pushed to GitHub.
10. Stop. Do not write application code. Do not start a new project. Wait for my instructions.

---

## FINAL CONSTRAINTS

- Do not write application code. This task is only to build the framework.
- Do not merge all design skills into a single generator.
- Respect the precedence rules. If in doubt, write the question under `Open Questions`; do not fill it with assumptions.
- Every file must be clean markdown, human-readable, and pushable to GitHub without losing functionality.
- Do not make exaggerated claims about Ponytail, unslop, or anti-slop in the README. Present the trade-offs honestly.
- The framework must be reinstallable into a fresh workspace and immediately usable via `/full-cycle <idea>`.
- **Every file, section heading, comment, frontmatter description, example text, and README paragraph MUST be written in English. Do not produce any Indonesian prose anywhere in the repository.**
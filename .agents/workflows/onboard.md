# Workflow: /onboard

Trigger: `/onboard`

Preliminary interactive onboarding wizard before running `/full-cycle` or `/auto-build`. Captures a clear project brief so planning begins without guesswork.

## Process

1. Explain to the user that `/onboard` collects project context up front before starting `/full-cycle` or `/auto-build`.
2. Ask the user these 3 simple questions and wait for their answers:
   - "What is the core idea of your app?"
   - "Do you have a specific tech stack in mind (e.g., Next.js, Tauri) or should I recommend one?"
   - "Do you have a design reference (Figma, URL, existing code) or should I generate one?"
3. Generate `.agents/PROJECT_BRIEF.md` summarizing their answers using this structure:
   ```markdown
   # Project Brief

   - **Core Idea:** [User's answer]
   - **Preferred Stack:** [Requested stack or "Recommend via stack-advisor / techspace-mcp"]
   - **Design Reference:** [Figma URL / Reference URL / Existing codebase path / "Generate via design-system-integrator Scenario D"]
   ```
4. Instruct the user:
   "Onboarding complete! When you are ready, run `/full-cycle` or `/auto-build` and I will use this brief to start building."

## Notes
- `.agents/PROJECT_BRIEF.md` feeds directly into Phase 1 (`prd-generator`), Phase 2 (`stack-advisor`), and Phase 3 (`design-system-integrator`).
- Safe to re-run `/onboard` whenever starting a new idea or updating preferences before Phase 1.

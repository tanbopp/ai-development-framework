# Workflow: /init-project

Trigger: `/init-project`

Prepares a fresh workspace. Safe to re-run: skip anything already present.

## Process
Platform note. Antigravity agents run commands in the shell of the host OS: bash on macOS and Linux, PowerShell on Windows. Prefer the agent's built-in file and git tools over shell commands where possible, so the workflow works on any platform. When shell commands are necessary, use the platform-appropriate block from the examples below.

1. Verify `.agents/rules/`, `.agents/agents/`, `.agents/skills/`, `.agents/platforms/`, `.agents/workflows/`, `.agents/SOUL.md`, and `docs/MCP_REGISTRY.md` exist, and confirm every rule has a valid `trigger` frontmatter.
   - Verify the five platform packs exist under `.agents/platforms/`: `web/SKILL.md`, `desktop/SKILL.md`, `mobile/SKILL.md`, `plugin/SKILL.md`, and `cli/SKILL.md`.
   - Verify the V3 skills exist under `.agents/skills/`: `database-architect/SKILL.md`, `security-hardening/SKILL.md`, and `mcp-discovery/SKILL.md`.
2. Prepare the vendor directories. The template ships each vendor folder with a `.gitkeep` placeholder. Before cloning, remove the placeholder:

   - Delete these four files if they exist: `.agents/vendor/ponytail/.gitkeep`, `.agents/vendor/design-taste-frontend/.gitkeep`, `.agents/vendor/hallmark/.gitkeep`, `.agents/vendor/unslop/.gitkeep`.
   - The agent should use its file-deletion tool, not a shell command, so this works on any platform.
   - Removing these files is safe because they only exist in the fresh template state. They are placeholders, not data.

   Then clone each vendor repo whose target directory does not already contain a `.git` subdirectory (safe to re-run). Continue on individual failure so one broken clone does not block the rest.

   Manual commands -- **bash / Git Bash / WSL**:
   ```bash
   rm -f .agents/vendor/ponytail/.gitkeep .agents/vendor/design-taste-frontend/.gitkeep .agents/vendor/hallmark/.gitkeep .agents/vendor/unslop/.gitkeep
   [ -d .agents/vendor/ponytail/.git ] || git clone --depth 1 https://github.com/DietrichGebert/ponytail.git .agents/vendor/ponytail || echo "WARN: ponytail clone failed"
   [ -d .agents/vendor/design-taste-frontend/.git ] || git clone --depth 1 https://github.com/changemaner/design-taste-frontend.git .agents/vendor/design-taste-frontend || echo "WARN: design-taste-frontend clone failed"
   ```

   Manual commands -- **PowerShell (Windows)**:
   ```powershell
   Remove-Item -Force -ErrorAction SilentlyContinue .agents/vendor/ponytail/.gitkeep, .agents/vendor/design-taste-frontend/.gitkeep, .agents/vendor/hallmark/.gitkeep, .agents/vendor/unslop/.gitkeep
   if (-not (Test-Path .agents/vendor/ponytail/.git)) { git clone --depth 1 https://github.com/DietrichGebert/ponytail.git .agents/vendor/ponytail }
   if (-not (Test-Path .agents/vendor/design-taste-frontend/.git)) { git clone --depth 1 https://github.com/changemaner/design-taste-frontend.git .agents/vendor/design-taste-frontend }
   ```
3. Install Hallmark (auditor and skill). Preferred:
   ```bash
   npx skills add nutlope/hallmark
   ```
   Alternative (only if the preferred method fails; the `.gitkeep` is already removed in Step 2):

   Clone `https://github.com/Nutlope/hallmark.git` into `.agents/vendor/hallmark`, but only if that directory does not yet contain a `.git` subdirectory.

   Manual command -- bash:
   ```bash
   [ -d .agents/vendor/hallmark/.git ] || git clone --depth 1 https://github.com/Nutlope/hallmark.git .agents/vendor/hallmark
   ```

   Manual command -- PowerShell:
   ```powershell
   if (-not (Test-Path .agents/vendor/hallmark/.git)) { git clone --depth 1 https://github.com/Nutlope/hallmark.git .agents/vendor/hallmark }
   ```

   Browser checks (`sloplint --render`) also need `npm i -g puppeteer-core` and Chrome; static checks run without them.
4. Install unslop (auditor and MCP server) without keeping its `AGENTS.md` preset or extra IDE configs:
   1. Read the current `AGENTS.md` content and hold it in memory (or write it to a temporary backup file `AGENTS.md.framework-backup`).
   2. Run:
      ```bash
      npx -y unslop-mcp@latest init
      ```
   3. Overwrite `AGENTS.md` with the saved framework content (this removes unslop's "Linear Precision" preset while keeping `.agents/skills/unslop/SKILL.md` and `.mcp.json` intact).
   4. Delete `AGENTS.md.framework-backup` if created.
   Note: unslop's 72 rules include subjective preferences. Audits must allow overrides justified in `docs/project/DESIGN.md`.

   ### Cleanup after unslop init
   unslop `init` writes many IDE-specific config files and appends a "Linear Precision" preset to `AGENTS.md`. The preset uses `#5e6ad2` (purple/indigo) and `Inter`, both of which are anti-patterns in `02-design-principles.md`. The framework's design authority is `design-taste-frontend` with tokens defined in `DESIGN.md`, not unslop's preset.

   After `unslop init` completes:

   1. Restore `AGENTS.md` to its pre-init content (the framework version, not the merged version). The unslop SKILL.md and `.mcp.json` remain in place; only the AGENTS.md directive is removed.
   2. Delete these files/directories if they exist (created by unslop init, not needed for Antigravity):
      - `CLAUDE.md`, `.claude/`, `.claude-plugin/`
      - `.cursor/`, `.cursorrules`
      - `.windsurfrules`, `.clinerules`, `.zed/`
      - `.github/copilot-instructions.md`
      - `opencode.json`, `skills-lock.json`
   3. Keep: `.agents/skills/unslop/SKILL.md`, `.mcp.json`
   4. Report which files were removed.
5. Do not modify vendor contents. Adapt only in the wrapper layer (`.agents/skills/`).
6. Print the readiness checklist. If any clone or install step fails, report which vendor repositories or tools are missing, continue with the remaining steps, and mark the failed items as unchecked in the readiness checklist. Do not silently proceed.

## Readiness checklist
- [ ] Rules present with valid frontmatter (5 files)
- [ ] `.agents/SOUL.md` present
- [ ] `.agents/platforms/` present (web, desktop, mobile, plugin, cli)
- [ ] Agents present (planner, architect, frontend, qa, devops)
- [ ] Skills present (13, including `database-architect`, `security-hardening`, `mcp-discovery`)
- [ ] `docs/MCP_REGISTRY.md` present
- [ ] `.agents/vendor/ponytail/` populated
- [ ] `.agents/vendor/design-taste-frontend/` populated
- [ ] Hallmark installed
- [ ] unslop initialized
- [ ] `AGENTS.md` restored to framework version (no unslop preset)
- [ ] unslop IDE config files cleaned up (see Cleanup section)
- [ ] `.github/workflows/pr-quality.yaml` present

## Notes
- `peakoss/anti-slop` needs no installation. It runs from `.github/workflows/pr-quality.yaml` on GitHub.
- Spec Kit and OpenSpec are not dependencies. Their flow is adopted in `full-cycle.md`.


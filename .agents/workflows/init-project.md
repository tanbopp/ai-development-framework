# Workflow: /init-project

Trigger: `/init-project`

Prepares a fresh workspace. Safe to re-run: skip anything already present.

## Process
1. Verify `.agents/rules/`, `.agents/agents/`, `.agents/skills/`, and `.agents/workflows/` exist and every rule has a valid `trigger` frontmatter.
2. Remove the placeholder `.gitkeep` files and clone the vendor repos (skip if already cloned, and continue on individual failure):
   ```bash
   # Remove placeholder .gitkeep files (safe to re-run)
   rm -f .agents/vendor/ponytail/.gitkeep
   rm -f .agents/vendor/design-taste-frontend/.gitkeep
   rm -f .agents/vendor/hallmark/.gitkeep
   rm -f .agents/vendor/unslop/.gitkeep

   # Clone vendor repos (skip if already cloned)
   if [ ! -d .agents/vendor/ponytail/.git ]; then
     git clone --depth 1 https://github.com/DietrichGebert/ponytail.git .agents/vendor/ponytail || echo "WARN: ponytail clone failed"
   fi
   if [ ! -d .agents/vendor/design-taste-frontend/.git ]; then
     git clone --depth 1 https://github.com/changemaner/design-taste-frontend.git .agents/vendor/design-taste-frontend || echo "WARN: design-taste-frontend clone failed"
   fi
   ```
   The `.gitkeep` files are placeholders committed to the template. They are removed before cloning because `git clone` refuses non-empty target directories. Removing them is safe because they only exist in the fresh template state.
3. Install Hallmark (auditor and skill). Preferred:
   ```bash
   npx skills add nutlope/hallmark
   ```
   Alternative (ensure `.agents/vendor/hallmark/.gitkeep` is removed first; Step 2 already removes it, and skip if already cloned):
   ```bash
   rm -f .agents/vendor/hallmark/.gitkeep
   if [ ! -d .agents/vendor/hallmark/.git ]; then
     git clone --depth 1 https://github.com/Nutlope/hallmark.git .agents/vendor/hallmark || echo "WARN: hallmark clone failed"
   fi
   ```
   Browser checks (`sloplint --render`) also need `npm i -g puppeteer-core` and Chrome; static checks run without them.
4. Install unslop (auditor and MCP server):
   ```bash
   npx -y unslop-mcp@latest init
   ```
   Note: unslop's 72 rules include subjective preferences. Audits must allow overrides justified in `docs/project/DESIGN.md`. `init` writes agent files (for example `AGENTS.md`, `.mcp.json`) and merges into existing ones; review the diff before committing.
5. Do not modify vendor contents. Adapt only in the wrapper layer (`.agents/skills/`).
6. Print the readiness checklist. If any clone or install step fails, report which vendor repositories or tools are missing, continue with the remaining steps, and mark the failed items as unchecked in the readiness checklist. Do not silently proceed.

## Readiness checklist
- [ ] Rules present with valid frontmatter (5 files)
- [ ] Agents present (planner, architect, frontend, qa, devops)
- [ ] Skills present (8)
- [ ] `.agents/vendor/ponytail/` populated
- [ ] `.agents/vendor/design-taste-frontend/` populated
- [ ] Hallmark installed
- [ ] unslop initialized
- [ ] `.github/workflows/pr-quality.yaml` present

## Notes
- `peakoss/anti-slop` needs no installation. It runs from `.github/workflows/pr-quality.yaml` on GitHub.
- Spec Kit and OpenSpec are not dependencies. Their flow is adopted in `full-cycle.md`.


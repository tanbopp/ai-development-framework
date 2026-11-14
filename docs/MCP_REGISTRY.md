# MCP Registry

Recommended Model Context Protocol (MCP) servers for this framework, grouped by phase and capability.

## How to Install & Configure

### Option 1: Antigravity Settings (`mcp_config.json`)
Add desired servers under `mcpServers` in your Antigravity MCP configuration (via **Manage MCP Servers -> View raw config** or `~/.gemini/antigravity/mcp_config.json`), then restart or refresh MCP servers:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "<package-name>"]
    }
  }
}
```

### Option 2: Direct `npx` Verification
Test whether a server package runs locally before adding it to your workspace or global config:

```bash
npx -y <package-name> --help
```

*(Note: verify each upstream repository's latest package name, runtime requirements, and any required environment variables before enabling it.)*

---

## Categories

### 1. Tech Stack Recommendation
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `techspace-mcp` | Evaluates PRD functional/non-functional constraints to suggest Pareto-optimal technology stacks in Phase 2 (`mcp-discovery`). | Register `techspace-mcp` in `mcp_config.json` via `npx` or its upstream binary. |

### 2. Design & Tokens
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `better-design` | Extracts design systems, color palettes, typography, and component tokens from reference systems in Phase 3 (`design-system-integrator`). | Register via `npx` in `mcp_config.json`. |
| `designlang` | Queries and exports structured design tokens and language specifications for the chosen stack. | Register via `npx` in `mcp_config.json`. |
| `inspo` | Discovers industry-aligned visual references when the PRD does not specify a UI library or brand aesthetic. | Register via `npx` in `mcp_config.json`. |

### 3. Visual Testing
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `vrt-mcp` | Compares built UI screenshots against Phase 1 reference images or baselines during Phase 7 (`design-audit`). | Register via `npx` in `mcp_config.json`; pair with Playwright/Puppeteer. |
| `zerovisual` | Automated visual diffing to catch layout shifts, contrast regressions, and >5% pixel mismatches. | Register via `npx` in `mcp_config.json`. |

### 4. Mobile & Desktop Testing
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `flutter_skill` | Drives and inspects Flutter mobile/desktop applications for automated UI verification alongside `.agents/platforms/mobile/` or `desktop/`. | Configure per the `flutter_skill` upstream instructions (Dart/Flutter SDK required). |

### 5. Security & Code Quality
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `semgrep-mcp` | Static security analysis (OWASP rules, injection sinks, secret patterns) to reinforce `security-hardening` in Phase 7. | Configure `semgrep-mcp` via `uvx` / `npx` or Semgrep CLI per upstream docs. |
| `slopgate` | Automated anti-slop and code-quality gating to complement `code-quality-gate`. | Register via `npx` in `mcp_config.json`. |

### 6. Memory & Context
| Server | Purpose | Typical Setup |
| --- | --- | --- |
| `memory-seed` | Git-native memory retrieval across codebase history and past architectural context in Phases 1-2 (`mcp-discovery`). | Register `memory-seed` in `mcp_config.json` per upstream docs (for example via `npx`). |

### 7. Design System & Visual Audit
| Category | Servers | Purpose | Typical Setup |
| --- | --- | --- | --- |
| **Figma Integration** | `figma-console-mcp`, `design-context-bridge` | Extract DTCG tokens, typography, and component specs from Figma URLs in Phase 3 (`design-system-integrator`, Scenario A). | Register via `npx` / upstream binary in `mcp_config.json` with Figma credentials. |
| **Token Extraction** | `@designmdcc/mcp`, `designlang` | Extract brand and URL design language and token specifications in Phase 3 (`design-system-integrator`, Scenario C). | Register via `npx` in `mcp_config.json`. |
| **Zero-Design Generation** | `standby-design-mcp` | Generate production-ready design systems from scratch based on PRD industry and tone when no design reference exists (Scenario D). | Register via `npx` in `mcp_config.json`. |
| **Component & Token Search** | `ds-pilot`, `brandrail`, `systembridge-mcp` | Search local codebases for existing components and tokens before Phase 6 UI generation to prevent duplication. | Register via `npx` in `mcp_config.json`. |
| **Deterministic Visual Audit** | `plumb-cli`, `onsystem`, `@usespectra/mcp` | Run deterministic pixel-perfect and token compliance audits against `DESIGN.md` in Phase 7 (`design-audit`). | Register via `npx` / CLI in `mcp_config.json`. |

### 8. Security & Compliance
| Category | Servers | Purpose | Typical Setup |
| --- | --- | --- | --- |
| **Threat Modeling** | `Aegis MCP` | Assess threat model completeness (`aegis_assess`) and verify code adherence (`aegis_review`) against `.agents/security-context.yaml` / `THREAT_MODEL.md` (`security-context-manager`). | Register `Aegis MCP` in `mcp_config.json` per upstream binary/package setup. |
| **SAST** | `Semgrep MCP` | Static Application Security Testing (`semgrep_scan` / `scan_directory`) for OWASP Top 10 and CWE Top 25 flaws (`sast-scanner`). | Register `Semgrep MCP` via `uvx` / `npx` or Semgrep CLI per upstream docs. |
| **SCA & Secrets** | `GoThreatScope`, `ironward`, `@infoinlet/mcp-codeaudit`, `GitHub MCP Server (Secret Scanning)` | Secret detection (Gitleaks integration) and SBOM/OSV.dev dependency vulnerability scanning (`sca-secret-scanner`). | Register via `npx` / Go binary / GitHub MCP config in `mcp_config.json`. |
| **DAST** | `VibeShift`, `BreachKit` | Dynamic security probing on live running endpoints for auth bypass, IDOR, validation gaps, and CORS flaws (`dast-scanner`). | Register `VibeShift` or `BreachKit` in `mcp_config.json`. |
| **Autonomous Fixing** | `security-mcp` | Autonomously patch verified SAST/SCA findings in the working tree and re-verify fixes (`security-gate`). | Register `security-mcp` in `mcp_config.json`. |
| **Proving Findings** | `Sabba` | Prove exploitability and triage real security findings before Phase 8 sign-off (`security-gate`). | Register `Sabba` in `mcp_config.json` per upstream docs. |

### 9. Interaction & Cognitive QA
| Tool / Server | Purpose | Typical Setup |
| --- | --- | --- |
| `designlang verify-ui <url> --brand DESIGN.md` | Renders built pages at `390` / `768` / `1280` and lists every drift from `DESIGN.md`; exits `1` on drift (CI gate). | Run `designlang verify-ui <url> --brand docs/project/DESIGN.md` via CLI or `designlang` MCP. |
| `designlang slop <url>` | Scores how generic the rendered page looks against 10 measured AI-slop tells. | Run `designlang slop <url>` during Phase 7 QA before sign-off. |
| `unslop-preflight` | Runs source-level UI slop checks (`node scripts/scan-ui-implementation.mjs src`) and viewport-fit checks (`node scripts/scan-viewport-fit.mjs http://localhost:3000`) for `ui-interaction-authority` and `viewport-qa`. | Install `unslop-preflight` scripts and run against `src` and the local dev server. |
| `squint-mcp` *(optional)* | Cross-references DOM/CSS with painted pixels to detect visual problems without a reference baseline. | Register `squint-mcp` in `mcp_config.json` per upstream docs. |

### Bundled Audit MCP (Installed by `/init-project`)
- `unslop` (`npx -y unslop-mcp@latest init`): Registers `.mcp.json` at the workspace root for the 72-rule Design Craft Engine used by `design-audit`.


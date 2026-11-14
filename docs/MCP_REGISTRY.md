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
| `better-design` | Extracts design systems, color palettes, typography, and component tokens from reference systems in Phase 3 (`design-system-applier`). | Register via `npx` in `mcp_config.json`. |
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

### Bundled Audit MCP (Installed by `/init-project`)
- `unslop` (`npx -y unslop-mcp@latest init`): Registers `.mcp.json` at the workspace root for the 72-rule Design Craft Engine used by `design-audit`.


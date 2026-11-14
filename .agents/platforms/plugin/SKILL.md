---
name: platform-plugin
description: Extension and plugin platform engineering rules for VS Code Extensions, Chrome Extensions, and Figma Plugins, covering manifest security, sandbox boundaries, and lifecycle management. Load when STACK.md targets a Plugin or Extension.
---

# Plugin & Extension Platform Pack

## Trigger
Loaded conditionally when `docs/project/STACK.md` targets a browser extension, IDE extension, or design tool plugin.

## Supported Targets
- VS Code Extensions, Chrome Extensions (Manifest V3), Figma Plugins.

## Engineering Rules
1. **Manifest Security:** Request only the minimum required permissions (`permissions`, `host_permissions`, activation scopes). Enforce strict Content Security Policy (CSP) and never use `eval`, remote script execution, or wildcard host permissions without explicit justification in `DECISIONS.md`.
2. **Sandbox & Context Isolation:** Keep privileged host APIs (VS Code workspace/fs, Chrome service worker/storage, Figma scene graph) isolated from webview/popup/UI iframes. Validate all `postMessage` / `onMessage` payloads at the boundary.
3. **Extension Lifecycle:**
   - VS Code: use targeted `activationEvents` (never `*` unless required), push all listeners/commands to `context.subscriptions` for clean `deactivate()` disposal.
   - Chrome MV3: assume service workers are ephemeral; persist state in `chrome.storage` rather than module globals.
   - Figma: handle `figma.closePlugin()` cleanly and guard against missing selection/node types.

## Testing
- Unit test message schemas and core transform logic in isolation; run host test runners (`@vscode/test-electron`, Chrome extension E2E via Playwright/Puppeteer) for integration paths.

## Prohibitions
- Using `eval()`, `new Function()`, or inline scripts in extension HTML/webviews.
- Requesting broad permissions (`<all_urls>`, `*`) when narrow host or active-tab permissions suffice.


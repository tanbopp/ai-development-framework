---
name: platform-desktop
description: Desktop platform engineering rules for Tauri v2 and Electron, covering IPC security, local filesystem safety, auto-updaters, code signing, and WebDriverIO/Rust testing. Load when STACK.md targets Desktop.
---

# Desktop Platform Pack

## Trigger
Loaded conditionally when `docs/project/STACK.md` targets a Desktop application.

## Supported Stacks
- Tauri v2 (Rust core + web frontend) and Electron (main/preload/renderer).

## Engineering Rules
1. **IPC Security:**
   - Treat the UI/renderer as untrusted. Validate and schema-check every IPC command payload on the native/main side.
   - Electron: enforce `contextIsolation: true`, `nodeIntegration: false`, `sandbox: true`, and expose only narrow, typed methods via `contextBridge`.
   - Tauri v2: configure minimal capability permissions (`capabilities/*.json`); never expose unrestricted shell or broad system commands.
2. **Local Filesystem Access:** Restrict reads/writes to explicit app-data directories or user-approved file dialogs. Canonicalize and validate paths to prevent path traversal (`..`).
3. **Auto-Updaters & Code Signing:** Require signed update artifacts (Tauri updater signatures / platform code signing for macOS notarization and Windows Authenticode) over HTTPS; never execute unsigned remote code.
4. **Offline & Lifecycle:** Handle window close/minimize, single-instance locking if required, and safe local storage shutdown without data corruption.

## Testing
- **Tauri Core:** `cargo test` for Rust commands, filesystem guards, and IPC state.
- **UI / Integration:** WebDriverIO (`tauri-driver` or Electron driver) for end-to-end desktop flows.

## Prohibitions
- Enabling `nodeIntegration: true` or disabling `contextIsolation` in Electron.
- Passing unsanitized renderer strings into filesystem paths or shell invocations.


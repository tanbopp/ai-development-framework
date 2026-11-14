---
name: platform-mobile
description: Mobile platform engineering rules for React Native (Expo) and Flutter, covering offline-first storage, push notifications, app store deployment, 44px touch targets, and Detox/Maestro testing. Load when STACK.md targets Mobile.
---

# Mobile Platform Pack

## Trigger
Loaded conditionally when `docs/project/STACK.md` targets a Mobile application (iOS / Android).

## Supported Stacks
- React Native (Expo) and Flutter.

## Engineering Rules
1. **Offline-First Storage:** Persist critical local state and pending mutations in SQLite or WatermelonDB (or Drift/sqflite in Flutter). Sync idempotently on reconnect with explicit conflict handling and optimistic UI rollback on failure.
2. **Push Notifications:** Request notification permissions contextually with clear user rationale; handle denied states gracefully; validate/refresh device push tokens server-side and never store secrets in notification payloads.
3. **Mobile UI/UX Ergonomics:** Enforce at least `44x44` px (or 44pt/48dp) touch targets for all interactive controls, respect safe-area insets (notches, home indicators), handle keyboard avoidance on forms, and support low-connectivity loading/error states.
4. **App Store Deployment:** Separate development/staging/production bundle IDs and signing configs (EAS Build / Xcode / Play App Signing); strip debug logs and test credentials from release builds; check required privacy manifests and permission descriptions.

## Testing
- **Unit:** Jest / Vitest (React Native) or `flutter test` (Flutter).
- **End-to-End:** Detox or Maestro (or `flutter_skill` MCP when available) for real device/simulator flows, offline transitions, and gesture navigation.

## Prohibitions
- Storing auth tokens or sensitive secrets in unencrypted plain preferences (`AsyncStorage` / `SharedPreferences` without SecureStore/Keychain/Keystore).
- Interactive buttons or list rows smaller than 44px touch targets.


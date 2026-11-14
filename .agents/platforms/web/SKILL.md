---
name: platform-web
description: Web platform engineering rules for Next.js (App Router), Vite, SvelteKit, and React, covering Server vs Client Components, SEO, Web Performance (Lighthouse), and Playwright/Vitest testing. Load when STACK.md targets Web.
---

# Web Platform Pack

## Trigger
Loaded conditionally when `docs/project/STACK.md` targets a Web application.

## Supported Stacks
- Next.js (App Router), React, Vite, SvelteKit.

## Engineering Rules
1. **Server vs Client Components:** Default to Server Components for data fetching, secrets handling, and static markup. Add client boundaries (`'use client'` or framework equivalent) only at interactive leaves requiring state, effects, or browser APIs.
2. **SEO & Metadata:** Define semantic landmarks (`main`, `nav`, `header`, `footer`), unique page titles, meta descriptions, canonical/OpenGraph tags where public pages exist, and clean URL routing.
3. **Web Performance (Lighthouse):** Avoid layout shifts (explicit image/media dimensions), lazy-load non-critical routes or heavy widgets, minimize client bundle bloat, and optimize web fonts (`font-display: swap`).
4. **Accessibility & Responsive Design:** Respect WCAG AA contrast, keyboard navigation, and mobile viewport fit without horizontal overflow.

## Testing
- **Unit / Component:** Vitest (business logic, state transitions, validation).
- **End-to-End:** Playwright (critical user flows, auth, navigation, mobile viewport).

## Prohibitions
- Shipping database queries, API secrets, or admin logic in client bundles.
- Marking an entire page tree as a Client Component without a specific interactivity reason.


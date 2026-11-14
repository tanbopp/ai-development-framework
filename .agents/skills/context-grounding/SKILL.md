---
name: context-grounding
description: Physically verifies imported modules, symbols, signatures, APIs, and component props in the codebase or installed packages before generating code, preventing hallucinated imports and calls.
version: 4.0.0
status: active
---

# Context Grounding

## Trigger
Before writing any code that imports a function, component, or library, or calls an API.

## Process
1. **Verify Target Existence:**
   - Before writing `import { X } from 'Y'` or calling `functionZ()`, use tools (`grep`, `glob`, file reads, or an active MCP codebase search tool) to physically verify that `X`, `'Y'`, or `functionZ` exists in the codebase or the installed `node_modules` (or the stack's installed dependencies).
2. **Verify Exact Interface:**
   - Inspect the target's definition or type declarations to verify the exact export name (named vs default), function signature, parameters, return type, API endpoint path, and component props.
3. **Stop on Missing Target:**
   - If the target does not exist, **STOP** immediately and do not write the import. Create the target first (within `boundary-guard` scope), or ask the user before proceeding or adding a dependency.

## Output Contract
Every import, symbol reference, component prop, library call, and API endpoint in generated code is backed by a verified definition in the codebase or installed dependencies.

## Checklist
- [ ] Target module, symbol, or library verified in codebase or installed `node_modules`
- [ ] Exact export style, signature, or component props confirmed from source/types
- [ ] Missing targets handled by stopping rather than guessing

## Prohibitions
- Never hallucinate API endpoints, component props, or library functions based on memory.
- Never write unverified imports or assume default vs named exports without checking the target definition.

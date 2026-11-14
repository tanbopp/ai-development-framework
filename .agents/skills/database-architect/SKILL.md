---
name: database-architect
description: Enforces database schema integrity, indexing, Supabase Row Level Security (RLS), and N+1 query prevention across SQL/ORM stacks. Use in Phase 4 (Technical Spec) and Phase 6 (Implementation).
version: 4.0.0
status: active
---

# Database Architect

## Trigger
Phase 4 (Technical Spec) and Phase 6 (Implementation).

## Process
1. Read `docs/project/STACK.md`, `docs/project/ARCHITECTURE.md`, and `.agents/skills/database-architect/references/db-patterns.md` (on-demand) for SQL indexing strategies, explicit query projections, N+1 prevention, transactions, and Supabase RLS templates.
2. Apply universal schema and query integrity across SQL/ORM implementations:
   - Require explicit primary keys and indexed foreign keys on every table.
   - Forbid `SELECT *` and N+1 query loops; use explicit column selection, eager loading, or joins.
   - Enforce RLS policies and safe `SECURITY DEFINER` search paths when targeting Supabase.
   - Wrap multi-step state mutations in explicit transactions.
3. Verify schema migrations and query patterns trace back to `REQ-XXX` and `docs/specs/*.md`.

## Output Contract
Schema definitions, indexes, RLS policies, and query patterns documented in `docs/specs/` (Phase 4) and implemented without `SELECT *` or N+1 loops (Phase 6).

## Checklist
- [ ] Read `.agents/skills/database-architect/references/db-patterns.md`
- [ ] Every table has a primary key and indexed foreign keys
- [ ] Zero `SELECT *` queries or N+1 query loops in production code paths
- [ ] Supabase RLS enabled with explicit policies when using Supabase

## Prohibitions
- Leaving Supabase tables without RLS enabled in production schemas.
- Issuing queries inside `for`/`map` loops over parent records.
- Storing derived balances without transactional consistency where required.

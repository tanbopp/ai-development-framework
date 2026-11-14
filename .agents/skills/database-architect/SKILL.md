---
name: database-architect
description: Enforces database schema integrity, indexing, Supabase Row Level Security (RLS), and N+1 query prevention across SQL/ORM stacks. Use in Phase 4 (Technical Spec) and Phase 6 (Implementation).
---

# Database Architect

## Trigger
Phase 4 (Technical Spec) and Phase 6 (Implementation).

## Process
1. Read `docs/project/STACK.md` and `docs/project/ARCHITECTURE.md` to identify the chosen database and ORM/client (for example Supabase, PostgreSQL, SQLite, Prisma, Drizzle, MongoDB).
2. **Universal Schema & Query Rules:**
   - Every table (or document collection) MUST define an explicit primary key (`id` or domain key).
   - Every foreign key column MUST have a corresponding database index to prevent full-table scans on joins and cascade deletes.
   - Forbid `SELECT *` (or unbounded full-document fetches) in production queries; select only the columns required by the contract.
3. **If using Supabase:**
   - Enforce Row Level Security (`ALTER TABLE ... ENABLE ROW LEVEL SECURITY`) on every application table with explicit `SELECT`/`INSERT`/`UPDATE`/`DELETE` policies.
   - Use `SECURITY DEFINER` functions (with an explicit `SET search_path = public` or tight schema path) for controlled public or cross-tenant lookups instead of weakening RLS.
4. **If using an ORM (Prisma, Drizzle, TypeORM, SQLAlchemy, etc.):**
   - Prevent N+1 query problems by enforcing eager loading (`include`, `with`, or batched `IN` queries / `JOIN`s) whenever loading parent records with child relations.
   - Wrap multi-step state mutations (for example stock decrements + ledger entries) in explicit database transactions.
5. Verify schema migrations, constraints, and query patterns trace back to `REQ-XXX` and `docs/specs/*.md`.

## Output Contract
- Schema definitions, RLS policies, indexes, and query patterns documented in `docs/specs/` (Phase 4) and implemented cleanly with no `SELECT *` or N+1 loops (Phase 6).

## Checklist
- [ ] Every table has a primary key
- [ ] Every foreign key is indexed
- [ ] Zero `SELECT *` queries in production code paths
- [ ] Supabase tables all have RLS enabled and explicit policies (if Supabase)
- [ ] ORM relational queries use eager loading / joins with zero N+1 loops

## Prohibitions
- Leaving Supabase tables without RLS enabled in production schemas.
- Issuing queries inside `for`/`map` loops over parent records.
- Storing derived balances without transactional consistency where required.


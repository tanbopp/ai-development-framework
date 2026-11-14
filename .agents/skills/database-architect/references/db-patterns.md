# Database Patterns & RLS Reference

Reference guide for `.agents/skills/database-architect/SKILL.md`. Read on-demand when designing schemas in Phase 4 (`docs/specs/*.md`) or writing migrations and queries in Phase 6.

## 1. Primary Keys & Indexing Strategy
- Every table or collection MUST define an explicit primary key (`id` UUID/ULID/BIGINT or explicit domain key).
- Index every foreign key column to avoid full-table scans on joins and `ON DELETE CASCADE` checks:
  ```sql
  CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    assignee_id UUID REFERENCES users(id) ON DELETE SET NULL,
    status TEXT NOT NULL DEFAULT 'open',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
  );

  CREATE INDEX idx_tasks_project_id ON tasks (project_id);
  CREATE INDEX idx_tasks_assignee_id ON tasks (assignee_id);
  CREATE INDEX idx_tasks_project_status_created ON tasks (project_id, status, created_at DESC);
  ```
- Align composite index column order with high-frequency equality predicates first (`project_id`, `status`) and range/sort columns last (`created_at DESC`).

## 2. Explicit Projections & N+1 Query Prevention
- Forbid `SELECT *` or unbounded `findMany()` without explicit column selection and pagination (`LIMIT` / cursor) in production paths.
- Prevent N+1 queries by eager-loading relations in one query or batching IDs (`IN (...)` / `ANY($1)`) instead of querying inside loops:
  ```ts
  // Bad: N+1 loop
  const projects = await db.project.findMany({ select: { id: true, name: true } });
  for (const project of projects) {
    const count = await db.task.count({ where: { projectId: project.id } });
  }

  // Good: Single relational or grouped query
  const projects = await db.project.findMany({
    select: {
      id: true,
      name: true,
      _count: { select: { tasks: true } },
    },
  });
  ```

## 3. Transactional Integrity
- Wrap multi-step state mutations (for example balance transfers, inventory decrements + order creation, or audit log + mutation pairs) in explicit atomic transactions:
  ```sql
  BEGIN;
  UPDATE wallets SET balance_cents = balance_cents - 500 WHERE id = $1 AND balance_cents >= 500;
  INSERT INTO ledger_entries (wallet_id, delta_cents) VALUES ($1, -500);
  COMMIT;
  ```
- Verify row-affected counts or optimistic version checks inside the transaction and roll back on mismatch.

## 4. Supabase Row Level Security (RLS) Patterns
- Enable RLS on every application table and define explicit per-operation policies (`SELECT`, `INSERT`, `UPDATE`, `DELETE`):
  ```sql
  ALTER TABLE tasks ENABLE ROW LEVEL SECURITY;

  CREATE POLICY "tasks_select_member"
    ON tasks FOR SELECT
    USING (
      EXISTS (
        SELECT 1 FROM project_members pm
        WHERE pm.project_id = tasks.project_id
          AND pm.user_id = auth.uid()
      )
    );

  CREATE POLICY "tasks_insert_member"
    ON tasks FOR INSERT
    WITH CHECK (
      EXISTS (
        SELECT 1 FROM project_members pm
        WHERE pm.project_id = tasks.project_id
          AND pm.user_id = auth.uid()
      )
    );
  ```
- When controlled cross-tenant or public aggregate lookups are needed, encapsulate them inside a `SECURITY DEFINER` function with an explicit tight `search_path`:
  ```sql
  CREATE OR REPLACE FUNCTION get_public_project_summary(p_slug TEXT)
  RETURNS TABLE (id UUID, name TEXT)
  LANGUAGE sql
  SECURITY DEFINER
  SET search_path = public
  AS $$
    SELECT p.id, p.name FROM projects p WHERE p.slug = p_slug AND p.is_public = true;
  $$;
  ```

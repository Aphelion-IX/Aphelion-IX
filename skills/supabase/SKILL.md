---
name: supabase
description: Use when working with a Supabase project — designing or changing database schema, writing migrations, debugging via logs/advisors, configuring Row Level Security, or wiring up client-side Supabase integration. Covers both local CLI-based development and the Supabase MCP tools.
---

# Supabase

Guidance for developing safely against Supabase (Postgres + Auth + Storage + Edge Functions).

## When to use

- Creating or altering tables, policies, or extensions
- Debugging a failing query, auth issue, or edge function
- Wiring up a client app to a Supabase project (URL + anon/publishable key)
- Writing or reviewing a migration

## Before making schema changes

1. Inspect existing structure first — `list_tables` (MCP) or `supabase db diff` / `\d` locally — before writing a migration blind.
2. Write schema changes as migrations, not ad-hoc `execute_sql` calls, so they're reproducible and reviewable.
3. Default new tables to Row Level Security **enabled**, with explicit policies — never ship a public table without RLS unless it's deliberately public read.

## Debugging

1. Start with `get_logs` and `get_advisors` (MCP) before changing anything — the advisors flag common security/perf issues (missing RLS, missing indexes) directly.
2. Reproduce locally with the Supabase CLI (`supabase start`, `supabase db reset`) when possible rather than iterating against the remote project.

## Client integration

- Use `get_project_url` and `get_publishable_keys` (MCP), or the dashboard, to get the client-safe URL/anon key. Never put the `service_role` key in client-side code — it bypasses RLS entirely.
- Keep `service_role` usage server-side only (edge functions, server routes, migrations).

## Environments

- **Local dev with filesystem/shell access**: prefer the Supabase CLI (`supabase`) for the local stack, migrations, and edge functions; run `npx skills add supabase/agent-skills` to pull in Supabase's own agent skill for deeper guidance, and prefer testing locally before touching the remote project.
- **Web-only/remote (no shell)**: rely on the Supabase MCP tools directly; treat `apply_migration` as applying straight to the remote project, so review the SQL carefully first.

## Notes

- Prefer `create_branch` (MCP) to test schema changes on a Supabase branch before merging into the main project, when branching is available.
- Re-run `get_advisors` after a migration to confirm it didn't introduce new lint/security warnings.

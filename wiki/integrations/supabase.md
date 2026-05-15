---
title: Supabase
type: integration
status: active
tags: [integrations, supabase, db, auth, storage]
updated: 2026-05-15
---

# Supabase

Managed PostgreSQL + Auth + Storage. The CRM's primary backend dependency.

## What we use

| Service | Usage |
| --- | --- |
| PostgreSQL | All application data |
| Auth | Staff login, B2B convenio login, optional client login |
| Storage | Pet photos (`mascotas` bucket), inventory photos, cemetery news images (`portal` bucket) |

## Connection

- **Pooled** (`DATABASE_URL`): used by the Next.js app in serverless functions — PgBouncer in transaction mode.
- **Unpooled** (`DATABASE_URL_UNPOOLED`): used by migration scripts and one-off Node.js scripts — direct connection required for DDL.

## Auth contexts

| Context | Mechanism |
| --- | --- |
| CRM staff | Supabase Auth, SSR cookies via `@supabase/ssr` |
| Client portal | `tokenPortal` UUID (primary) + optional Supabase Auth |
| B2B convenio portal | Supabase Auth (invited by admin), separate login at `/acceso` |

## Row-Level Security

RLS is disabled. All queries run server-side with the service role key. `requireAuth()` is the security boundary, not RLS.

## Storage buckets

| Bucket | Contents | Public? |
| --- | --- | --- |
| `mascotas` | Pet photos and gallery images | Yes (public URLs in memorial) |
| `portal` | Cemetery news images | Yes |
| `inventario` | Inventory item photos | Yes |

## Local development

Each developer runs a local Supabase instance via the Supabase CLI (Docker-based). Config at `crm/supabase/config.toml`.

**Note:** `[analytics]` is disabled in `config.toml` — the analytics container requires Docker daemon exposed on TCP 2375 (Windows-only constraint).

### Workflow

```bash
# Start local stack (runs on 127.0.0.1:54321)
cd crm && supabase start

# Apply all migrations from scratch
supabase db reset

# Seed your local user (admin, role: admin)
node scripts/seed-user-local.mjs

# Stop when done
supabase stop
```

### Local credentials (CRM `.env.local`)

Pull production env vars from Vercel first (`vercel link && vercel env pull .env.local`), then replace the 5 Supabase lines with local values:

- `NEXT_PUBLIC_SUPABASE_URL` → `http://127.0.0.1:54321`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` → anon key from `supabase start` output
- `SUPABASE_SERVICE_ROLE_KEY` → service role key from `supabase start` output
- `DATABASE_URL` → local postgres URL on port 54322 (postgres/postgres)
- `DATABASE_URL_UNPOOLED` → same as `DATABASE_URL` locally

### Seed user

`scripts/seed-user-local.mjs` creates a Supabase Auth user + `usuarios` row with role `admin` and all permissions. Credentials are in the script.

## Plan

Supabase Pro — $25/month. Includes PITR (Point-in-Time Recovery) for disaster recovery.
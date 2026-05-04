---
title: Supabase
type: integration
status: active
tags: [integrations, supabase, db, auth, storage]
updated: 2026-05-04
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

## Plan

Supabase Pro — $25/month. Includes PITR (Point-in-Time Recovery) for disaster recovery.
---
title: Autenticación del CRM
type: security
status: active
tags: [security, auth, crm]
updated: 2026-05-04
---

# Autenticación del CRM

Three independent auth mechanisms, each covering a different surface.

## 1. Staff login (Supabase Auth + optional 2FA)

- Email + password via Supabase Auth.
- On success, Supabase sets an SSR cookie (`sb-*`).
- Every server route and server component calls `requireAuth(roles)` before doing anything. Unauthenticated requests are redirected to `/auth/login`.
- Optional second factor: email OTP (see [[wiki/security/2fa-otp]]).

## 2. Client portal (tokenPortal)

- UUID stored in `clientes.token_portal`.
- Sent to the client in the invitation email.
- The URL `/portal/[token]` is the credential — no login required.
- Server reads the token, looks up the client, and renders the portal.
- Optional Supabase Auth layer for future features requiring verified identity (`esClienteLogueado` prop).

## 3. B2B convenio portal (Supabase Auth)

- Invited via `POST /api/portal/convenio/invitar` which creates a Supabase Auth user.
- Login at `/acceso` (separate from staff login).
- On login, resolves `convenios.auth_user_id` → `convenios.token_portal` → redirects to `/portal/convenio/[token]`.

## Route protection

```ts
// Every protected API route:
const auth = await requireAuth(['admin', 'manager'])
if (!auth.ok) return auth.response

// Every protected page:
const session = await requireAuth(['admin'])
if (!session.ok) redirect('/auth/login')
```

`requireAuth` is in `crm/src/lib/api-auth.ts`. It reads the Supabase SSR session and checks the role against the allowed list.

## Row-Level Security

RLS is disabled on the CRM database (`postgres` role, no anon access). All DB access goes through the server with the service role key. RLS is therefore not the primary security boundary — `requireAuth` is.
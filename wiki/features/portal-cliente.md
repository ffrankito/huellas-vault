---
title: Portal Cliente
type: feature
status: active
tags: [portal, v2]
updated: 2026-05-04
---

# Portal Cliente

Self-service portal for pet owners. Accessible at `/portal/[tokenPortal]`. No login required — the token in the URL is the primary auth mechanism. See [[wiki/decisions/2026-04-portal-auth-dual-mechanism]].

## Access

Two independent mechanisms:

| Mechanism | Path | Required? |
|-----------|------|-----------|
| `tokenPortal` in URL | `/portal/[token]` | Primary — always works |
| Supabase Auth (email+password) | `/portal/login` | Optional |

Invitation flow: admin sends invite → client gets email with `/portal/activar?token=X` → sets password → Supabase Auth user created.

## Tabs

1. **Servicios** — list of services with current state.
2. **Planes** — cuotas paid, current coverage %, plan state.
3. **Memorial** — per-pet bottom sheet: photo, dates, dedicatoria, gallery. Editable without login (`EditarMemorialInline`).
4. **Novedades** — cemetery news where `publicada = true`, ordered by `destacada DESC, creado_en DESC`.

## Memorial editing

- Client edits `dedicatoria` and uploads photos to gallery from the portal.
- Setting `memoriaPublica = true` makes the pet visible at `/memorial/[mascotaId]`.
- Editing requires only the tokenPortal — no Supabase Auth needed.

## Logout

`LogoutBtn` component → `supabase.auth.signOut()` → redirect to `/auth/login`.

## Key routes

| Route | Description |
|-------|-------------|
| `/portal/login` | Login with email/password |
| `/portal/activar` | Account activation (from invitation email) |
| `/portal/[token]` | Portal home with 4 tabs |
| `/memorial` | Public grid of all active memorials |
| `/memorial/[mascotaId]` | Individual public memorial (dark aesthetic) |
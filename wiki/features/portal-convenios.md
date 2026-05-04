---
title: Portal Convenios B2B
type: feature
status: active
tags: [portal, convenios, v2, b2b]
updated: 2026-05-04
---

# Portal Convenios B2B

Partner portal for veterinary clinics, petshops, refuges, and other B2B partners. Accessible at `/portal/convenio/[tokenPortal]`. Allows partners to submit leads directly into the CRM kanban.

## Access

| Mechanism | Path | Required? |
|-----------|------|-----------|
| `tokenPortal` in URL | `/portal/convenio/[token]` | Primary — always works |
| Supabase Auth (email+password) | `/portal/convenio/login` | Optional — via email invite |

Admin activates portal: `portalActivo = true` on the convenio. Optionally sends email invite → `POST /api/portal/convenio/invitar` → creates Supabase Auth user.

## Lead submission

Partner fills form: nombre, teléfono, **DNI** (required for B2B), mascota, tipo de servicio.

Creates `POST /api/leads` with:
- `origen: "veterinaria"`
- `veterinariaId: <convenioId>`
- `dni: <value>` (field added to leads schema for B2B)

Lead appears in CRM kanban with the partner's label. At conversion, `descuentoPorcentaje` from the convenio is applied automatically to the service.

## Features shown to partner

- Form to submit new lead.
- Table of leads submitted by this partner (last 50).
- Logout button.

## Known gaps

- No pagination on leads table (hardcoded to 50). See [[wiki/gaps/portal-convenios-pagination]].
- No notification to partner when lead status changes. See [[wiki/gaps/portal-convenios-notifications]].

## Key routes

| Route | Description |
|-------|-------------|
| `/portal/convenio/login` | Login for invited partners |
| `/portal/convenio/[token]` | Partner portal home |

## Key API routes

| Route | Description |
|-------|-------------|
| `POST /api/portal/convenio/invitar` | Creates Supabase Auth user for partner |
| `GET /api/portal/convenio/mi-token` | Resolves tokenPortal after login |
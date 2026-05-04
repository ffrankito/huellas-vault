---
title: Novedades del Cementerio
type: feature
status: active
tags: [novedades, portal, crm, v1]
updated: 2026-05-04
---

# Novedades del Cementerio

Cemetery news managed by admins in `/dashboard/novedades`. Visible to clients in their portal under the Novedades tab.

## States

| Field | Default | Meaning |
|-------|---------|---------|
| `publicada` | `true` | `false` = draft, hidden from portal |
| `destacada` | `false` | `true` = pinned to top |

## Ordering

`ORDER BY destacada DESC, creado_en DESC` — pinned items first, then newest.

## CRM management

- `NovedadCard` — Client Component with optimistic UI for `publicada`/`destacada` toggles (no loading flicker).
- `NuevaNovedadForm` — create modal with "Publicar ahora / Guardar como borrador" toggle.
- `EditarNovedadBtn` — edit text + image.
- `EliminarNovedadBtn` — confirm dialog.

Images stored in Supabase Storage at `portal/novedades/`.

## Portal display

Only `publicada = true` news shown. Client cannot edit, only read.
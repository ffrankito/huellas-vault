---
title: Leads — Kanban y Gestión
type: feature
status: active
tags: [leads, crm, v1]
updated: 2026-05-04
---

# Leads — Kanban y Gestión

**Diagrams:** [[wiki/architecture/plantuml/04-estado-lead]] · [[wiki/architecture/plantuml/05-seq-convertir-lead]]

Lead pipeline from capture to conversion. Agents work from `/dashboard/mis-leads` (one lead at a time queue) while admins/managers see the full kanban at `/dashboard/leads`.

## States

```
nuevo → contactado → interesado → cotizado → convertido
                                           ↘ perdido
```

## Capture channels

| Origin | `origen` value | Notes |
|--------|----------------|-------|
| Cotizador iframe | `cotizador` | Includes `pickupMethod` |
| Landing contact form | `landing` | |
| B2B portal | `veterinaria` | Includes `veterinariaId` + `dni` |
| Excel bulk import | `base_propia` | Via `/dashboard/configuracion/importar-leads` |
| Manual (CRM) | `directo` | |

## Cron archiving (`/api/cron/leads` — every hour)

| Condition | Action |
|-----------|--------|
| `nuevo` + no activity for 72h | → `perdido` |
| `interesado` + no activity for 48h | → `perdido` |
| `perdido` + 10 days old | → deleted |

## Mis-leads queue (`/dashboard/mis-leads`)

One lead at a time. Features:
- Stopwatch running while lead is open.
- WhatsApp popup (pre-filled message, stays in CRM tab).
- Integrated email send (Resend).
- Programar seguimiento — lead disappears from queue until `seguimientoEn` is reached.
- Mandatory report before moving to next lead.
- Conversion modal → creates Cliente + Mascota + Plan or Servicio.

## Lead assignment

New leads are automatically assigned to the `televenta` agent with the lowest current load at creation time. Goal is fast response, not strict equality.

- Transfer between agents requires a mandatory reason, recorded in `lead_interacciones`
- Manual reassignment is admin/manager only
- B2B leads (`origen: 'veterinaria'`) arrive pre-flagged as high priority and appear first in the agent queue

## Bulk import

Upload `.xlsx` → `POST /api/leads/importar`. Deduplication by phone. Returns summary: imported / duplicates / errors. Creates `ImportacionLead` record.

## Permissions

- `televenta`: sees and manages own leads + `mis-leads` queue.
- `admin` / `manager`: see all leads, can reassign, have full edit access.
- Lead transfer requires mandatory reason (recorded in `lead_interacciones`).
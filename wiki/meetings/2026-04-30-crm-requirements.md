---
title: CRM Requirements — Leads, Planes, Convenios
type: meeting
status: done
tags: [meeting, crm, leads, planes, convenios]
date: 2026-04-30
participants: [Tomás Pinolini, Franco Zancocchia, Lucía, Dalma, Martina]
---

# CRM Requirements — Leads, Planes, Convenios

**Date:** 2026-04-30
**Participants:** Tomás, Franco, Lucía, Dalma, Martina

---

## Context

Deep-dive requirements session for the CRM. Agents Dalma and Martina participated directly — the goal was to validate workflows before building. Most of what was agreed here maps 1:1 to the Fase 1a/1b implementation.

---

## Key decisions

### Client table visibility

Agents need to see the full client table — not just their own — so they can:
- Verify clients are correctly loaded
- Monitor team load and avoid duplicates

Financial data is **not** shown in this table. Financial management stays in Jaque Mate. Lucía reviews new client records monthly.

### Lead lifecycle

States agreed in this meeting:

```
nuevo → contactado → interesado → cotizado → convertido
                                           ↘ perdido
```

Each state transition requires a note. Agents cannot move to the next lead without reporting on the current one (mandatory report gate). This came directly from Lucía's concern about accountability — see [[wiki/features/leads-kanban]].

### Round-robin lead assignment

New leads are automatically assigned to the agent with the lowest current load. Not strictly equal distribution — the goal is efficient response, not identical counts. Fabian's user was to be removed from rotation (confirmed in this meeting).

### Lead transfer audit trail

Any lead transfer between agents must be documented with a reason. This prevents "the lead disappeared" situations during vacations or absences. Transfers are recorded in `lead_interacciones`.

### Veterinary portal: high-priority leads

Vets enter leads directly via the B2B portal. These are flagged as high-priority and land at the top of the agent queue. Commercial negotiation with vets stays personal (off-platform) — the portal is only for lead submission and tracking, not for surfacing discount terms.

### Financial separation

The CRM handles sales and lead management. Billing and plan activation remain in Jaque Mate. The agreed integration approach:
- Agents complete the sale in CRM
- Monthly export from CRM → import into Jaque Mate (manual, not real-time)
- Mercado Pago payment links deferred to a later phase (not in v1)

**Why:** Attempting real-time sync with Jaque Mate in v1 would have created dependency on their system and risked blocking the launch. The export button covers the monthly reconciliation need.

### Plan Plus for large dogs

Dogs over 25 kg require a surcharge (Plan Plus). This is **internal only** — not shown on the landing or cotizador. The agent applies it during the sale based on declared weight. Keeping it off the public site avoids surprising clients with unexpected charges post-contact.

### Permissions per role

| Role | Can see | Can edit |
|---|---|---|
| `televenta` | Own leads + full client table | Own leads only |
| `admin` / `manager` | Everything | Everything |
| `transporte` | Own agenda, assigned services | Service status |
| `cremacion` | Services in cremation | Service status |
| `entrega` | Services ready for delivery | Service status |

Agents cannot modify plan configurations — that's admin-only.

### Data quality: mandatory fields

Agents cannot complete a client record without: DNI, phone, address, pet name, pet species. Enforced at the form level — no soft warnings.

### Export for Jaque Mate sync

A one-click export of all client/plan data in a format compatible with Jaque Mate's import. This covers the monthly reconciliation workflow without requiring real-time integration.

---

## Action items (as of meeting — all resolved)

- ✅ Full client table visible to all agents
- ✅ Lead states: nuevo → contactado → interesado → cotizado → convertido / perdido
- ✅ Round-robin lead assignment
- ✅ Mandatory note on lead transfer (recorded in `lead_interacciones`)
- ✅ Veterinary portal with high-priority leads
- ✅ Role-based permissions
- ✅ Mandatory fields enforced on client/pet forms
- ✅ Export button for Jaque Mate monthly sync
- ⏳ Mercado Pago payment link integration (deferred)
- ⏳ Plan Plus (>25kg dogs) — needs confirmation of final pricing

---

## Open questions at time of meeting

- Exact fields and format for Jaque Mate export (→ Lucía to confirm)
- Mercado Pago scope and timeline (→ deferred)
- Certificate of cremation format (→ carried over from kickoff)

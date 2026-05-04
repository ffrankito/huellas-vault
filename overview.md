---
title: Overview
description: Top-level synthesis of the Huellas de Paz project. Homepage of the vault.
updated: 2026-05-04
status: active
---

# Huellas de Paz — Overview

Huellas de Paz is the first formally licensed pet crematory in Rosario, Santa Fe, Argentina, with over 20 years in the industry. This vault is the private knowledge base behind the software project: decisions, synthesis, meeting notes, and context that doesn't belong in the shipped repo.

**Built by Ravenna** — Tomás Pinolini (product/dev lead) + Franco Zancocchia (tech lead).

## People

| | Who | What they own |
|---|---|---|
| Product lead | [[wiki/people/tomas-pinolini]] | Scope, roadmap, client relationship, vault curation |
| Tech lead | [[wiki/people/franco-zancocchia]] | CRM, portals, landing, cotizador — all implementation |

## What exists today (Mayo 2026)

All phases 0–3 are implemented and in production.

- **Landing (Fase 0)** — Astro 6 static site. Sections: Hero, Servicios, Planes, Memoriales, Convenios, Contacto, Ubicación. Formulario de contacto → CRM. Navbar with "Ingresar" dropdown.
- **Cotizador (Fase 0)** — React + Vite iframe. 8-step quote flow. Prices fetched live from CRM `servicios_config`. Currently hidden in landing until crematory opens.
- **CRM v1 (Fases 1a + 1b)** — Next.js 15, Drizzle ORM, Supabase. Full operations: clients, pets, services, prepaid plans, leads kanban, agenda, inventory, B2B agreements, reports, comms, AI assistant.
- **Portal cliente (Fase 2)** — Token-URL portal. Tabs: Servicios, Planes, Memorial, Novedades. Memorial editable without login.
- **Memorial público (Fase 2)** — `/memorial/[mascotaId]` for pets with `memoria_publica = true`. Dark aesthetic.
- **Portal convenios B2B (Fase 3)** — Vet/petshop partner portal. Lead submission, history view. Login optional (token URL sufficient).
- **Seguridad 2FA** — Email OTP (opt-in per user). SHA-256 hash, 10-min expiry, 3-attempt limit, 8-hour MFA session cookie.

## What's pending

### Blocking for launch
- Resend domain config — emails currently send from `onboarding@resend.dev`. Must verify own domain.
- WhatsApp number — currently placeholder `5493XXXXXXXXX` in landing.
- Logo final — placeholder active.

### Client content
- Plan names and final prices (servicios_config is loaded; planes_config needs client approval).
- Real facility photos (currently stock).
- Client testimonials.
- Activate cotizador once crematory opens.

### Integrations
- Mercado Pago — env vars present, implementation pending.

### Future
- **Fase 4** — WhatsApp + Instagram chatbot (not before Fases 0–3 are stable). See [[wiki/gaps/chatbot-fase-4]].

## Load-bearing constraints

- **Supabase as sole DB** — Drizzle ORM, no raw `pg`. Manual migrations after 0009 via Node.js scripts. Never use `drizzle-kit push` in prod (destructive column detection).
- **No Mercado Pago yet** — all payment-related features are behind env vars not yet configured.
- **veterinarias alias** — `convenios` table is exported as both `convenios` and `veterinarias` for legacy code. New code uses `convenios` only.
- **tokenPortal** — primary auth for both portals. Supabase Auth is secondary, optional.

## Navigation

- **[[index]]** — catalog of all wiki pages by type.
- **[[wiki/architecture/plantuml/index|Architecture]]** — system models, use cases, deployment.
- **[[log]]** — chronological record. Most recent entries at the bottom.
- **[[CLAUDE]]** — the schema that keeps the LLM disciplined.
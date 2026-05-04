---
title: Index
description: Catalog of all wiki pages. LLM-maintained — updated on every ingest.
updated: 2026-05-04
---

# Index

Catalog of all `wiki/` pages, organized by type. One-line description per entry.

> [!info] Maintenance
> Update this file on every ingest. Each entry: `[[wiki/path]] — one-line summary (updated YYYY-MM-DD)`.

---

## People

- [[wiki/people/tomas-pinolini]] — product/dev lead, vault curator, Ravenna co-founder (2026-05-04)
- [[wiki/people/franco-zancocchia]] — tech lead, full-stack implementation, Ravenna co-founder (2026-05-04)

## Versions

- [[wiki/versions/v1-v2]] — Fases 0–3: Landing, Cotizador, CRM, Portal Cliente, Portal Convenios — all shipped (2026-05-04)
- [[wiki/versions/v3]] — Fase 4: WhatsApp + Instagram chatbot — planned, not started (2026-05-04)

## Features

- [[wiki/features/2fa-otp]] — Email OTP two-factor auth, opt-in per user, 8h session cookie (2026-05-04)
- [[wiki/features/portal-cliente]] — Token-URL client portal: Servicios · Planes · Memorial · Novedades (2026-05-04)
- [[wiki/features/portal-convenios]] — B2B partner portal for vets/petshops to submit leads (2026-05-04)
- [[wiki/features/leads-kanban]] — Lead pipeline: kanban, cron archiving, bulk import, mis-leads queue (2026-05-04)
- [[wiki/features/asistente-ia]] — Internal Claude Haiku assistant with rate limiting and budget cap (2026-05-04)
- [[wiki/features/memorial-digital]] — Public pet memorial page at /memorial/[mascotaId] (2026-05-04)
- [[wiki/features/novedades-cementerio]] — Cemetery news with draft/published/pinned states (2026-05-04)
- [[wiki/features/cotizador]] — Embeddable 8-step price calculator, prices from CRM servicios_config (2026-05-04)

## Decisions

- [[wiki/decisions/2026-04-2fa-email-otp]] — Chose email OTP over TOTP app for 2FA; simpler UX for the team (2026-05-04)
- [[wiki/decisions/2026-04-portal-auth-dual-mechanism]] — tokenPortal URL as primary auth, Supabase Auth as optional secondary (2026-05-04)
- [[wiki/decisions/2026-04-convenios-renamed]] — Renamed `veterinarias` table to `convenios`; alias maintained for legacy code (2026-05-04)
- [[wiki/decisions/2026-03-stack]] — Next.js 15 + Drizzle + Supabase for CRM; Astro for landing; Vite/React for cotizador (2026-05-04)

## Gaps / open questions

- [[wiki/gaps/mercadopago-pending]] — Env vars present but MP not integrated; online payment blocked (2026-05-04)
- [[wiki/gaps/chatbot-fase-4]] — WhatsApp + Instagram chatbot planned but not started (2026-05-04)
- [[wiki/gaps/resend-domain]] — Emails from `onboarding@resend.dev`; own domain not verified yet (2026-05-04)
- [[wiki/gaps/cotizador-hidden]] — Cotizador commented out in landing until crematory opens officially (2026-05-04)
- [[wiki/gaps/whatsapp-number]] — Placeholder `5493XXXXXXXXX` in landing; real number pending from client (2026-05-04)
- [[wiki/gaps/client-content]] — Logo, real photos, testimonials, plan names — all pending from client (2026-05-04)

## Architecture

- [[wiki/architecture/plantuml/index]] — Índice oficial de arquitectura PlantUML: actores, casos de uso, clases, estados, secuencias, deployment (2026-05-04)
- [[wiki/architecture/plantuml/01-actores]] — Actores primarios (7 roles staff + cliente), secundarios (Supabase, Resend, Vercel Cron) (2026-05-04)
- [[wiki/architecture/plantuml/02-casos-de-uso]] — Use case overview: captación, leads, operaciones, convenios, portales (2026-05-04)
- [[wiki/architecture/plantuml/03-clases-dominio]] — Domain class model: core entities + support entities + enums (2026-05-04)
- [[wiki/architecture/plantuml/04-estados]] — State machines: Lead, Servicio, Plan, Convenio, Sesión MFA (2026-05-04)
- [[wiki/architecture/plantuml/05-secuencias]] — Key sequence diagrams: cotizador→lead, conversión, cremación multi-rol, 2FA (2026-05-04)
- [[wiki/architecture/plantuml/06-deployment]] — C4 Deployment: Vercel × 3, Supabase, Resend, Anthropic (2026-05-04)

## Security

- [[wiki/security/auth-crm]] — CRM auth: Supabase SSR + requireAuth() helper + role/permission model (2026-05-04)
- [[wiki/security/2fa-otp]] — 2FA deep-dive: hash, cookie, session lifecycle, skipMfa option (2026-05-04)

## Integrations

- [[wiki/integrations/supabase]] — PostgreSQL + Auth + Storage; Drizzle ORM; manual migration pattern post-0009 (2026-05-04)
- [[wiki/integrations/resend]] — Transactional email; currently from onboarding@resend.dev; domain pending (2026-05-04)
- [[wiki/integrations/vercel]] — Hosting for all 3 apps + cron jobs (2026-05-04)

## Archive

_Empty — nothing archived yet._
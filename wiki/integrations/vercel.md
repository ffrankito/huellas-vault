---
title: Vercel
type: integration
status: active
tags: [integrations, vercel, hosting, cron]
updated: 2026-05-04
---

# Vercel

Hosting platform for all four apps. Pro plan — $20/month.

## Deployed apps

| App | Directory | URL pattern |
| --- | --- | --- |
| CRM | `crm/` | `huellasde-paz.vercel.app` |
| Landing | `landing/` | `huellasdepaz.vercel.app` |
| Cotizador | `cotizador/` | `cotizador-huellas.vercel.app` |
| Chatbot | `chatbot/` | Not deployed (Fase 4) |

## Cron jobs (CRM)

Defined in `crm/vercel.json`. Runs on the CRM's serverless functions.

| Job | Schedule | Route |
| --- | --- | --- |
| Lead automation | Daily | `GET /api/cron/leads` |

## Environment variables

Set per-project in the Vercel dashboard. The CRM has the largest set — see `CLAUDE.md` for the full list including pending vars (`MP_*`, `RESEND_FROM_EMAIL`).

## Deployment

Auto-deploy on push to `main`. Each app in the monorepo is a separate Vercel project with its own root directory setting.
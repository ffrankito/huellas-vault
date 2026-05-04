---
title: Resend — dominio propio pendiente
type: gap
status: active
tags: [email, gap, resend]
updated: 2026-05-04
---

# Resend — dominio propio pendiente

All transactional emails currently send from `onboarding@resend.dev`. This is a Resend sandbox address not suitable for production.

## Affected files

- `crm/src/lib/email/invitacion.tsx`
- `crm/src/lib/email/estadoServicio.ts`
- `crm/src/lib/email/enviarEmailLead.ts`
- `crm/src/app/api/portal/recuperar/route.ts`

## What's needed to unblock

1. Client configures a domain in Resend (e.g. `huellasdepaz.com.ar`).
2. Add DNS records (SPF, DKIM, DMARC) as instructed by Resend.
3. Update `from:` in the four files above to e.g. `Huellas de Paz <no-reply@huellasdepaz.com.ar>`.
4. Set `RESEND_FROM_EMAIL` env var in Vercel.

## Risk if not resolved

Emails land in spam or are blocked entirely. Portal invitations and service status notifications will fail silently for many clients.
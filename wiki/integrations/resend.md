---
title: Resend
type: integration
status: active
tags: [integrations, resend, email]
updated: 2026-05-04
---

# Resend

Transactional email provider. Used for all outbound emails from the CRM.

## Emails sent

| Trigger | Template | Recipients |
| --- | --- | --- |
| Client portal invitation | `lib/email/invitacion.tsx` | Client |
| Service status change | `lib/email/estadoServicio.ts` | Client |
| Lead follow-up | `lib/email/enviarEmailLead.ts` | Lead |
| Password recovery (portal) | `api/portal/recuperar/route.ts` | Client |
| 2FA OTP code | Inline in auth flow | Staff user |

## Current state

Sending from `onboarding@resend.dev` (sandbox). See [[wiki/gaps/resend-domain]] — production domain not yet configured.

## Plan

Free tier — 3,000 emails/month. Sufficient for current volume.

## Env vars

```
RESEND_API_KEY
RESEND_FROM_EMAIL   # pending — set once domain is verified
```
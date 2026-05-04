---
title: 2FA — Email OTP
type: feature
status: active
tags: [security, auth, v2]
updated: 2026-05-04
---

# 2FA — Email OTP

Opt-in two-factor authentication for CRM staff. Each user activates it individually from `/dashboard/perfil`. Uses emailed 6-digit OTP codes, not a TOTP app.

## Design decision

See [[wiki/decisions/2026-04-2fa-email-otp]].

## How it works

1. User logs in normally with Supabase Auth (email + password).
2. If `mfaEmailActivo = true`, `GET /api/me` returns `{ mfaRequerido: true }`.
3. Browser redirects to `/auth/verificar-mfa`.
4. Page auto-sends OTP: `POST /api/auth/otp/enviar` (bypasses MFA check via `skipMfa: true`).
5. Resend delivers a 6-digit code by email.
6. User submits code → `POST /api/auth/otp/verificar`.
7. API verifies SHA-256 hash, sets `mfa_s` cookie (`userId:token`, httpOnly, 8h).
8. Browser redirects to `/dashboard`.

## Code constraints

- OTP routes use `requireAuth(undefined, { skipMfa: true })` — otherwise chicken-and-egg (you'd need MFA to complete MFA).
- `requireAuth()` in `crm/src/lib/api-auth.ts` validates the `mfa_s` cookie on every protected request.
- OTP hash stored as `SHA-256(codigo + userId)` — never the plaintext code.

## Expiry and limits

| Parameter | Value |
|-----------|-------|
| OTP validity | 10 minutes |
| Max attempts | 3 before lockout |
| MFA session duration | 8 hours |
| Lockout recovery | Request a new code |

## DB columns (on `usuarios`)

```
mfa_email_activo  boolean   default false
otp_codigo        text      SHA-256 hash, null when no active OTP
otp_expira_en     timestamp
otp_intentos      integer   default 0
mfa_sesion_token  text
mfa_sesion_expira_en timestamp
```

Migration: `crm/scripts/migrate-0015.mjs`.

## User flow to activate

From `/dashboard/perfil` → Seguridad → Activar 2FA:
1. Enter current password (Supabase re-auth).
2. Send OTP to verify identity.
3. Enter OTP → `mfaEmailActivo = true`.

Same flow in reverse to deactivate.

## Component

`Configuracion2FA` in `crm/src/components/configuracion/Configuracion2FA.tsx`.
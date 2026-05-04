---
title: 2FA — implementación Email OTP
type: security
status: active
tags: [security, auth, 2fa]
updated: 2026-05-04
---

# 2FA — implementación Email OTP

Technical detail of the email OTP second factor for CRM staff.

## Flow

1. Staff enters email + password → Supabase Auth validates credentials.
2. If `usuarios.mfa_email_activo = true`:
   - Generate 6-digit code.
   - Store `SHA-256(code + userId)` in `usuarios.otp_codigo`.
   - Set `usuarios.otp_expira_en = now() + 10 minutes`.
   - Reset `usuarios.otp_intentos = 0`.
   - Send code via Resend to the user's email.
   - Redirect to `/auth/verificar-mfa`.
3. User enters the code.
4. Server verifies: `SHA-256(input + userId) === otp_codigo` AND `otp_expira_en > now()` AND `otp_intentos < 3`.
5. On success: set `mfa_sesion_token` (random UUID) + `mfa_sesion_expira_en = now() + 8h` in the DB. Set `mfa_s` cookie.
6. On failure: increment `otp_intentos`. At 3 attempts → block (return 429, require new login).

## Schema fields

| Field | Type | Purpose |
| --- | --- | --- |
| `mfa_email_activo` | boolean | Whether 2FA is enabled for this user |
| `otp_codigo` | text | SHA-256 hash of the current OTP |
| `otp_expira_en` | timestamp | Expiry (10 min from issue) |
| `otp_intentos` | integer | Failed attempts counter |
| `mfa_sesion_token` | text | Random UUID stored after successful MFA |
| `mfa_sesion_expira_en` | timestamp | 8h session window |

## Security properties

- Code is never stored in plaintext — only its hash.
- Codes expire in 10 minutes.
- 3 failed attempts lock the flow (must re-login to get a new code).
- MFA session is separate from the Supabase Auth session — both must be valid.

## Decision rationale

See [[wiki/decisions/2026-04-2fa-email-otp]].
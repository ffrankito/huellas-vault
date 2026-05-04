---
title: 2FA — Email OTP over TOTP
type: decision
status: active
tags: [security, auth]
updated: 2026-05-04
---

# 2FA: Email OTP over TOTP app

**Decision:** implement two-factor authentication using emailed 6-digit OTP codes, not a TOTP authenticator app (Google Authenticator, Authy, etc.).

## Context

CRM staff need an optional second factor to protect admin accounts. The team is small (< 10 people), non-technical, and already uses email.

## Rationale

- **Simpler UX** — no app to install, no QR code setup. Team uses email daily.
- **Lower support surface** — no "I lost my phone" recovery edge case with a TOTP secret.
- **Sufficient threat model** — the main risk is password reuse. Email OTP stops that without adding friction that would lead the team to leave 2FA disabled.
- **Acceptable tradeoff** — email OTP is weaker than TOTP (email account compromise = MFA bypass), but the team's email accounts are also protected and this is an internal ops tool, not a financial system.

## Implementation

See [[wiki/features/2fa-otp]] for full technical detail.

## What we rejected

- **TOTP (authenticator app)** — stronger security, but setup complexity and recovery scenarios not worth it for this team size and threat model.
- **SMS OTP** — requires a telecom provider/cost, Resend already exists.
- **Magic links** — already how the client portal works; conflating the mechanisms would confuse staff vs client auth.
---
title: Portal Auth — Dual Mechanism (token + Supabase)
type: decision
status: active
tags: [auth, portal, v2]
updated: 2026-05-04
---

# Portal Auth: tokenPortal URL as primary, Supabase Auth as optional secondary

Both the client portal and the B2B convenio portal use this pattern.

**Decision:** `tokenPortal` (UUID in the URL) is the primary and always-sufficient auth mechanism. Supabase Auth (email + password) is a secondary, optional layer.

## Context

Pet owners need to access their portal. Most will arrive via a link from the invitation email. Requiring a password creates a drop-off point for clients who are already grieving.

## Rationale

- **Frictionless access** — client clicks the link in the email and lands directly in their portal. No login step.
- **Security is adequate** — the token is a UUID (cryptographically random, unguessable). The URL itself is the credential.
- **Email = already verified** — the token was sent to the client's email address by the admin, so email delivery acts as identity confirmation.
- **Supabase Auth for future features** — if features requiring verified identity are added (e.g., payments), the client already has an optional account. The portal can gate specific actions behind `esClienteLogueado` without forcing everyone to create an account.

## Applied to

- **Portal cliente** (`/portal/[token]`) — `esClienteLogueado` prop passed from server for future use.
- **Portal convenios** (`/portal/convenio/[token]`) — same pattern.

## What we rejected

- **Login-only portal** — too much friction for grieving clients. Drop-off risk too high.
- **Magic link on every visit** — adds latency; token-in-URL is simpler and already works.
---
title: Mercado Pago — integración pendiente
type: gap
status: active
tags: [payments, gap]
updated: 2026-05-04
---

# Mercado Pago — integración pendiente

Online payment for services and plan cuotas is not implemented. The infrastructure is ready but the client has not provided credentials.

## Current state

- Env vars `MP_ACCESS_TOKEN`, `MP_PUBLIC_KEY`, `MP_WEBHOOK_SECRET` are defined in the schema but not set in Vercel.
- No Mercado Pago SDK calls exist in the codebase.
- Plan cuotas are registered manually by staff (`cuotasPagadas + 1`).

## What's needed to unblock

1. Client creates and provides their Mercado Pago production credentials.
2. Set env vars in Vercel (CRM project).
3. Implement payment links for cuotas (`/api/pagos/mp`).
4. Configure MP webhook at `/api/pagos/mp-webhook`.
5. Test with real transactions.

## Estimated scope

Approximately 1–2 weeks of dev work once credentials are available. See [[wiki/versions/v1-v2]] for original scope note.
---
title: Cotizador — oculto hasta apertura del crematorio
type: gap
status: active
tags: [cotizador, gap, landing]
updated: 2026-05-04
---

# Cotizador — oculto hasta apertura del crematorio

The `<Cotizador />` section is commented out in `landing/src/pages/index.astro`. The cotizador app itself is fully built and deployed.

## Current state

```astro
{/* <Cotizador /> */}
```

## What's needed to unblock

1. Client confirms the crematorio is operational and ready to receive online leads.
2. Uncomment `<Cotizador />` in `landing/src/pages/index.astro`.
3. Verify the iframe embed URL points to the deployed cotizador on Vercel.
4. Deploy the landing.

## Note

The cotizador sends leads to `POST /api/leads` with `origen: 'cotizador'`. The CRM already handles this origin correctly.
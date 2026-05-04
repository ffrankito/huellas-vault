---
title: Cotizador
type: feature
status: active
tags: [cotizador, landing, v1]
updated: 2026-05-04
---

# Cotizador

Embeddable 8-step price calculator. Deployed as a standalone React + Vite app, embedded as an iframe in the landing page. **Currently hidden** in the landing until the crematory opens officially. See [[wiki/gaps/cotizador-hidden]].

## Steps

1. Pet type (canino / felino / otro).
2. Size/weight — only for caninos (pequeño / mediano / grande / extra grande).
3. Service — prices loaded live from CRM.
4. Pickup method (domicilio / crematorio).
5. Ashes delivery (inmediata / diferida).
6. Zone — only if domicilio.
7. Contact data → `POST /api/leads (origen: cotizador)`.
8. Success screen.

## Live pricing

On mount, fetches `GET /api/configuracion/servicios` (CORS-enabled). Maps by `tipo`:

| `tipo` | Maps to |
|--------|---------|
| `cremacion_comunitaria` | Plan Huellitas |
| `cremacion_individual` (cheaper) | Plan Compañeros |
| `cremacion_individual` (pricier) | Plan Siempre Juntos |
| `entierro` | Jardín del Recuerdo |

Falls back to hardcoded values if the API call fails.

## Env var

`VITE_CRM_BASE_URL` — points to the CRM. Defaults to `https://huellasde-paz.vercel.app`.
---
title: Asistente IA Interno
type: feature
status: active
tags: [ai, crm, v1]
updated: 2026-05-04
---

# Asistente IA Interno

Internal Claude Haiku assistant available to all CRM staff. Answers questions about the business using context about Huellas de Paz. Accessible from a floating chat widget in the dashboard.

## Architecture

- Model: `claude-haiku-4-5-20251001` (configurable via `ASISTENTE_MODELO` env var).
- Route: `POST /api/asistente/chat` — accepts `{ pregunta, screenContext }`.
- Budget cap: $10 USD/month (configurable via `ASISTENTE_PRESUPUESTO_USD`).
- Rate limit: 5 requests/minute per user (configurable via `ASISTENTE_RATE_LIMIT_POR_MINUTO`).

## Audit

Admin-only audit page at `/dashboard/asistente`:
- All queries logged in `asistente_log` table.
- Columns: `usuarioId`, `rol`, `pregunta`, `screenContext`, `tokensInput`, `tokensOutput`.
- Accumulated cost calculated from token counts.

## Limits

| Guard | Behavior |
|-------|----------|
| Rate limit reached | 429 — "límite por minuto alcanzado" |
| Budget exhausted | 403 — "presupuesto mensual agotado" |

## DB table

`asistente_log` — all queries persisted for auditing. Monthly cost is `SUM(tokensInput * price_in + tokensOutput * price_out)` calculated at query time.
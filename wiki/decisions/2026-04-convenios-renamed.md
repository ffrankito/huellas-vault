---
title: Tabla veterinarias renombrada a convenios
type: decision
status: active
tags: [db, schema, convenios]
updated: 2026-05-04
---

# Tabla `veterinarias` renombrada a `convenios`

**Decision:** rename the `veterinarias` table to `convenios` to reflect the broader scope of B2B partners beyond just vet clinics.

## Context

The B2B module was initially designed only for veterinary clinics. After design review it became clear that petshops, refuges, and other businesses would also use the same module.

## Result

- DB table: `convenios`
- Schema exports: both `convenios` (canonical) and `veterinarias` (alias for legacy code)
- Field name `clientes.veterinariaId` **NOT renamed** — would require a migration that touches foreign keys, not worth the risk. The field name is legacy; the actual FK points to `convenios.id`.

## Rule for new code

Always use `convenios` in new code. `veterinarias` alias is only for files that were written before the rename.

## Migration

Applied in migration 0009 via `drizzle-kit`.
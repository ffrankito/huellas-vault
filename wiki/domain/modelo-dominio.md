---
title: Modelo de dominio
type: domain
status: active
tags: [domain, entities, relationships]
updated: 2026-05-06
---

# Modelo de dominio

![[03-clases-dominio]]

---

## Entidades principales

| Entidad | Tabla DB | Descripción |
|---|---|---|
| `Usuario` | `usuarios` | Miembro del equipo interno con rol y permisos |
| `Cliente` | `clientes` | Dueño de mascota — tiene `token_portal` único para el portal |
| `Mascota` | `mascotas` | Mascota del cliente — puede tener galería y memorial público |
| `Servicio` | `servicios` | Cremación o entierro — ciclo de vida de 5 estados |
| `Plan` | `planes` | Plan de previsión contratado — cobertura diferida por cuotas |
| `Lead` | `leads` | Prospecto — ciclo de vida de 6 estados hasta conversión |
| `Convenio` | `convenios` | Socio B2B (veterinaria, petshop, refugio) — tiene portal propio |
| `Inventario` | `inventario` | Urnas, bolsas, accesorios — con stock mínimo y alertas |

## Relaciones clave

- Un `Cliente` tiene muchas `Mascota`, muchos `Servicio` y muchos `Plan`
- Un `Lead` puede convertirse en un `Cliente` (y crea `Mascota` + opcionalmente `Servicio`)
- Un `Servicio` puede estar asociado a un `Convenio` (con descuento)
- Un `Convenio` puede originar muchos `Lead`

## Ciclos de vida

| Entidad | Diagrama de estados |
|---|---|
| Servicio | [[wiki/architecture/plantuml/04-estado-servicio]] |
| Lead | [[wiki/architecture/plantuml/04-estado-lead]] |
| Plan | [[wiki/architecture/plantuml/04-estado-plan]] |
| Sesión MFA | [[wiki/architecture/plantuml/04-estado-mfa]] |

## Roles

| Rol | Acceso |
|---|---|
| `admin` | Todo el sistema |
| `manager` | Reportes, equipo, rendimiento |
| `contadora` | Reportes financieros, cobranzas |
| `televenta` | Leads, clientes, planes |
| `transporte` | Agenda, servicios asignados |
| `cremacion` | Servicios en cremación |
| `entrega` | Servicios listos para entregar |

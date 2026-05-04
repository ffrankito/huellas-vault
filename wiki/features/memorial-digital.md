---
title: Memorial Digital
type: feature
status: active
tags: [memorial, portal, v2]
updated: 2026-05-04
---

# Memorial Digital

Public tribute pages for deceased pets. Visible at `/memorial/[mascotaId]` when `memoriaPublica = true` on the mascota record.

## Public grid

`/memorial` — grid of all active public memorials. No auth required.

## Individual memorial

`/memorial/[mascotaId]` — dark aesthetic (`#08080f` background — the only dark page in the system). Shows:
- Hero photo.
- Pet name, species, dates (born/died).
- Dedicatoria (text written by the owner).
- Photo gallery.

SEO: `generateMetadata` generates title/description per pet.

## Editing (from portal)

Owner edits memorial from `/portal/[token]` → Memorial tab → `EditarMemorialInline`.
- No Supabase Auth required — tokenPortal is sufficient.
- Can toggle `memoriaPublica` on/off.
- Upload photos to gallery (Supabase Storage `portal/` bucket).

## Privacy

- Default: `memoriaPublica = false` — visible only within the client portal.
- When `memoriaPublica = true`: indexed in `/memorial` grid and shareable via direct URL.
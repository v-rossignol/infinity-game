---
title: Scout-X1
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - vehicules
  - scout-x1
sources:
  - ../../../contracts/units.md
---

# `scout-x1` — Scout-X1

**Implemented** · `vehicule` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `true` |
| Speed | `1` (one hex per 2 min at planet surface — see [contracts/game-rules.md](../../../contracts/game-rules.md) → Vehicule speed) |
| Environments | `plain`, `forest`, `mountain`, `desert`, `ice`, `volcanic` (all `HEX_BIOMES` except `ocean`) |
| Rules | `{ range: hexagon, value: 1 }` — toroidal move-order range |
| Recipe | — (not buildable; granted on first spawn) |

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `5`, types `["*"]` |
| `building` | speed `1`, buildings: sizes `["small"]`, units `["*"]` |

Description: *Can explore, extract, and build small structures.*

## Related

- [index.md](index.md) — vehicules catalog
- [../capabilities.md](../capabilities.md) — capability key definitions

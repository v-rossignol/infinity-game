---
title: Starter grants
author: Composer
created: 2026-07-09
updated: 2026-07-09
status: draft
tags:
  - spawn
  - units
  - scout-x1
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/units.md
  - ../../okf/units/vehicules/scout-x1.md
---

# Starter grants

On **first spawn**, the server grants starting assets outside the normal unit **build** flow. Direct instance creation on `enter-game` is a separate server path from recipe-based construction.

## Granted at spawn (implemented)

| Grant | Details |
| ----- | ------- |
| `scout-x1` (Scout-X1) | One planet-level vehicule on the home planet |

No buildings and no star-system units are granted at spawn.

### Scout-X1

| Field | Value |
| ----- | ----- |
| Unit id | `scout-x1` |
| Category | `vehicule` · `small` |
| Recipe | — (**not buildable**; granted on first spawn only) |
| `placeLevel` | `planet` |
| Solaris view access | **Does not** grant access — Solaris requires a unit at `placeLevel = starSystem` |

Full type definition: [../units/vehicules/scout-x1.md](../units/vehicules/scout-x1.md) · authoritative catalog: [contracts/units.md](../../contracts/units.md).

### Capabilities (starter vehicule)

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `5`, types `["*"]` |
| `building` | speed `1`, buildings: sizes `["small"]`, units `["*"]` |

The Scout-X1 can explore, extract any terrain resource on its hex, and build small structures. Harvested resources depend on the spawn hex biome — see [../resources/terrain-resources.md](../resources/terrain-resources.md).

## Not granted at spawn

| Topic | Notes |
| ----- | ----- |
| Buildings | Freshy owns no buildings at spawn; `sawmill` and `quarry` require recipes |
| Star-system units | Required for Solaris `can-enter`; not part of first spawn |
| Pre-filled cargo | Not defined in contracts |
| Terrain resources | Hex resources come from biome rules, not a spawn bonus |

## Status

| Topic | Implemented | Planned |
| ----- | ----------- | ------- |
| Grant `scout-x1` on first spawn | Yes | — |
| Additional starter units or resources | — | Not specified |

## Related

- [first-entry.md](first-entry.md) — when grants are applied
- [home-planet.md](home-planet.md) — where the starter vehicule is placed
- [../resources/units-and-resources.md](../resources/units-and-resources.md) — extraction and recipes from the resources perspective
- [index.md](index.md) — spawn domain overview

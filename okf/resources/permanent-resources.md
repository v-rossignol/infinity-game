---
title: Permanent resources
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - resources
  - terrain
  - permanent
sources:
  - ../../contracts/resources.md
  - ../../packages/shared-config/src/terrain-resources.ts
---

# Permanent resources

**Permanent** terrain resources are always present on every hex of matching terrain. They are resolved at request time from the hex biome and returned by `GET /infinity/resources/planet/{planetId}/hex/{q}/{r}`.

Canonical constants live in `PERMANENT_TERRAIN_RESOURCES` in [packages/shared-config/src/terrain-resources.ts](../../packages/shared-config/src/terrain-resources.ts); the server re-exports them from `infinity/src/shared/constants/terrain-resources.constants.ts`.

Planet surface generation stores `resources: []` on every hex; the hex resources endpoint does not read persisted hex resources.

Each resource has a stable **`id`** (kebab-case). The hex endpoint maps `id` → `type`, `quantity` → `abundance`, and sets `rarity` to `common` for all permanent terrain resources today.

## Catalog

One row per terrain–resource pair. Quantities match `PERMANENT_TERRAIN_RESOURCES` in shared-config.

| Id | Name | Terrain | Quantity |
| --- | --- | --- | --- |
| `food` | Food | `plain` | 10 |
| `fresh-water` | Fresh water | `plain` | 10 |
| `wood` | Wood | `forest` | 50 |
| `food` | Food | `forest` | 5 |
| `stone` | Stone | `mountain` | 75 |
| `iron-ore` | Iron ore | `mountain` | 10 |
| `copper-ore` | Copper ore | `mountain` | 10 |
| `coal` | Coal | `mountain` | 5 |
| `silica` | Silica | `desert` | 100 |
| `alkaline-minerals` | Alkaline minerals | `desert` | 5 |
| `food` | Food | `ocean` | 30 |
| `salt-water` | Salt water | `ocean` | 100 |
| `ice` | Ice | `ice` | 100 |
| `cryogenic-materials` | Cryogenic materials | `ice` | 5 |
| `obsidian` | Obsidian | `volcanic` | 10 |
| `sulfur` | Sulfur | `volcanic` | 10 |
| `basalt` | Basalt | `volcanic` | 10 |

## Status

| Topic | Status |
| ----- | ------ |
| Constants in shared-config | **Implemented** |
| Hex resources API resolution by biome | **Implemented** |
| Resource harvesting gameplay | **Planned** |

## Related

- [occasional-resources.md](occasional-resources.md) — terrain resources present only on some hexes
- [transformed-resources.md](transformed-resources.md) — products from transformations (planned)
- [terrain-resources.md](terrain-resources.md) — biomes and terrain resource model
- [contracts/resources.md](../../contracts/resources.md) — authoritative catalog (all categories)

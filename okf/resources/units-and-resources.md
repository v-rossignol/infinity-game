---
title: Units and resources
author: Composer
created: 2026-07-06
updated: 2026-07-06
status: draft
tags:
  - resources
  - units
  - extraction
  - recipes
sources:
  - ../../contracts/units.md
  - ../../contracts/game-rules.md
---

# Units and resources

Units interact with terrain resources through **extraction** (harvesting from hex cells) and **recipes** (building structures from cargo-held resources).

Resource ids in capabilities and recipes match the terrain catalog in [contracts/resources.md](../../contracts/resources.md).

## Extraction capability

Units with an `extraction` capability harvest resources on the planet surface:

| Field | Meaning |
| ----- | ------- |
| `speed` | Extraction rate multiplier |
| `types` | Allowed resource ids, or `"*"` for any resource present in the hex biome |

### Seeded unit types (implemented)

| Unit id | Name | Category | Extraction |
| ------- | ---- | -------- | ---------- |
| `scout-x1` | Scout-X1 | `vehicule` | speed `5`, types `["*"]` |
| `sawmill` | Sawmill | `building` | speed `1`, types `["wood"]` |
| `quarry` | Quarry | `building` | speed `1`, types `["stone"]` |

The Scout-X1 can extract any terrain resource available on its current hex. Sawmill and Quarry specialize in `wood` and `stone` respectively, matching their build environments (`forest` and `mountain`).

## Build recipes

Buildable units consume **ingredients** from the builder's cargo when a build order starts. Ingredient keys are terrain resource ids.

### Seeded recipes (implemented)

| Unit id | Name | Ingredients | Work |
| ------- | ---- | ----------- | ---- |
| `sawmill` | Sawmill | `wood` × 25, `stone` × 100 | 100 |
| `quarry` | Quarry | `wood` × 100, `stone` × 50 | 150 |

`scout-x1` is not buildable — it is granted on first player spawn.

## Cargo

Units with a `cargo` capability store harvested or transported resources. All three seeded types (`scout-x1`, `sawmill`, `quarry`) have cargo size `1000`.

## Place level

Unit instances are stored at a **place level** (`cube`, `starSystem`, or `planet`). Seeded types with extraction operate at **planet** surface depth today.

## Full unit catalog

Capabilities, environments, movement rules, and planned types:

→ [contracts/units.md](../../contracts/units.md)

Movement timing on the planet surface (relevant when units travel between hexes to extract):

→ [contracts/game-rules.md](../../contracts/game-rules.md) → Vehicule speed

## Related

- [terrain-resources.md](terrain-resources.md) — biomes and terrain resource categories
- [index.md](index.md) — resources domain overview

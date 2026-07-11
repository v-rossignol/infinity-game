---
title: Terrain resources
author: Composer
created: 2026-07-06
updated: 2026-07-08
status: draft
tags:
  - resources
  - terrain
  - biomes
  - hexagons
sources:
  - ../../contracts/game-rules.md
---

# Terrain resources

Terrain resources are harvested from **planetary hex cells**. Each hex belongs to one biome; resources available on that hex depend on its biome.

## Hex biomes

Planet surfaces use a toroidal hex grid. Each hexagon has one of these biomes:

| Biome | Id |
| ----- | -- |
| Plain | `plain` |
| Forest | `forest` |
| Ocean | `ocean` |
| Mountain | `mountain` |
| Desert | `desert` |
| Ice | `ice` |
| Volcanic | `volcanic` |

See [contracts/game-rules.md](../../contracts/game-rules.md) → Hexagons and Surface topology for grid layout.

## Resource categories

Two categories of **terrain resources** exist by hex biome:

| Category | Description |
| -------- | ----------- |
| **Permanent** | Always present on every hex of matching terrain — see [permanent-resources.md](permanent-resources.md) |
| **Occasional** | Present only on some hexagons of matching terrain *(planned — procedural placement not implemented)* — see [occasional-resources.md](occasional-resources.md) |
| **Transformed** | Products from transformations, not harvested from terrain *(planned)* — see [transformed-resources.md](transformed-resources.md) |

Each resource has a stable **`id`** (kebab-case) for contracts, constants, and persistence.

## Gameplay status

| Topic | Status |
| ----- | ------ |
| Hex biomes on planet surfaces | **Implemented** |
| Permanent resource resolution by biome | **Implemented** (see [contracts/resources.md](../../contracts/resources.md)) |
| Occasional resource placement | **Planned** |
| Resource harvesting gameplay | **Planned** (part of the broader colonization / crafting vision in [contracts/game-rules.md](../../contracts/game-rules.md)) |

## Catalogs by category

| Category | OKF document | Contract source |
| -------- | ------------ | ----------------- |
| Permanent | [permanent-resources.md](permanent-resources.md) | [contracts/resources.md](../../contracts/resources.md) |
| Occasional | [occasional-resources.md](occasional-resources.md) | [contracts/resources.md](../../contracts/resources.md) |
| Transformed | [transformed-resources.md](transformed-resources.md) | [contracts/resources.md](../../contracts/resources.md) |

## Related

- [units-and-resources.md](units-and-resources.md) — which units extract or require which resource ids
- [index.md](index.md) — resources domain overview

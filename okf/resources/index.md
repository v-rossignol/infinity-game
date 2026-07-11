---
title: Resources
author: Composer
created: 2026-07-06
updated: 2026-07-09
status: draft
tags:
  - resources
  - terrain
  - biomes
  - units
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/units.md
---

# Resources
Knowledge domain for **terrain resources** on planetary hex cells — how they relate to biomes and gameplay, and how units extract or consume them.

## Purpose

Organize terrain resource catalogs by category and document how they relate to biomes, units, and gameplay.

## Documents

| Document | Description |
| -------- | ----------- |
| [terrain-resources.md](terrain-resources.md) | Hex biomes and the permanent vs occasional terrain resource model |
| [permanent-resources.md](permanent-resources.md) | Terrain resources always present on matching hex biomes |
| [occasional-resources.md](occasional-resources.md) | Terrain resources on some hexes only *(planned placement)* |
| [transformed-resources.md](transformed-resources.md) | Products from resource transformations *(planned)* |
| [units-and-resources.md](units-and-resources.md) | Unit extraction capabilities and build recipes that reference resource ids |

## Authoritative catalogs

| Document | Role |
| -------- | ---- |
| [contracts/game-rules.md](../../contracts/game-rules.md) | Places, hexagons, player presence, movement |
| [contracts/units.md](../../contracts/units.md) | Unit type catalog, capabilities, recipes |

## Related

- [units/](../units/) — unit types, catalog, and capabilities
- [OKF root](../index.md)
- [OKF reference](../reference/okf-spec.md)

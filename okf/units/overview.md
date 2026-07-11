---
title: Units overview
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - unit-type
  - unit-instance
sources:
  - ../../contracts/units.md
---

# Units overview

## UnitType and UnitInstance

| Concept | Description |
| ------- | ----------- |
| **`UnitType`** | Immutable unit template — defines id, category, size, capabilities, recipe |
| **`UnitInstance`** | Runtime object owned by a player, created from a `UnitType` |

Each unit type has a stable **`id`** (kebab-case) for contracts, constants, and persistence. The human-readable **Name** is separate from the id.

## Categories and sizes

| Category | Id | Mobility |
| -------- | -- | -------- |
| Vehicule | `vehicule` | Can move |
| Building | `building` | Fixed at a location |

| Size | Id |
| ---- | -- |
| Small | `small` |
| Medium | `medium` |
| Large | `large` |

## Catalog pipeline

Canonical seed data lives in `infinity/src/modules/units/constants/unit-catalog.ts` (`UNIT_CATALOG`).

On server startup, `UnitCatalogService.ensureCatalog()` upserts each definition into the PostgreSQL `unit_type` table. Runtime reads use the database; the TypeScript constants are the authoring source.

Recipe **ingredients** and extraction **types** reference terrain resource ids from [contracts/resources.md](../../contracts/resources.md).

## Status legend

- **Implemented** — seeded in `UNIT_CATALOG` and enforced by Infinity Server today.
- **Planned** — design intent; not in the catalog seed yet.

| Topic | Implemented | Planned |
| ----- | ----------- | ------- |
| Catalog seed | `scout-x1`, `sawmill`, `quarry` | Additional vehicules and buildings |
| Planet surface | Move, extract, build, cargo, park/unpark | — |
| Place levels | `planet` instances for seeded types | `cube`, `starSystem` vehicules |
| Inter-place travel | — | Cube ↔ system ↔ planet movement |

Unit instances are stored at a **place level**: `cube`, `starSystem`, or `planet` (`placeLevel` on unit instances). First spawn creates a planet-level vehicule on the home planet.

## Related

- [vehicules/](vehicules/) and [buildings/](buildings/) — seeded unit type entries
- [capabilities.md](capabilities.md) — capability key reference
- [../resources/units-and-resources.md](../resources/units-and-resources.md) — extraction and recipes from the resources perspective
- [index.md](index.md) — units domain overview

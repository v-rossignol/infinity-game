---
title: Units
author: Composer
created: 2026-07-08
updated: 2026-07-14
status: draft
tags:
  - units
  - catalog
  - vehicules
  - buildings
sources:
  - ../../contracts/units.md
---

# Units

Knowledge domain for **`UnitType`** definitions and **`UnitInstance`** runtime objects in Infinity.

## Purpose

Summarize the unit catalog from the contract draft and link to authoritative sources in `contracts/` and server code. Detailed seed values match `UNIT_CATALOG` in the server constants.

## Documents

| Document | Description |
| -------- | ----------- |
| [overview.md](overview.md) | UnitType vs UnitInstance, categories, sizes, implementation status |
| [capabilities.md](capabilities.md) | Capability keys (`cargo`, `extraction`, `building`, `garage`) |

## Catalog by category

| Domain | Description |
| ------ | ----------- |
| [vehicules/](vehicules/) | Mobile unit types (Scout-X1) |
| [buildings/](buildings/) | Immobile unit types (Sawmill, Quarry, Forge, Dock) |

## Authoritative sources

| Document | Role |
| -------- | ---- |
| [contracts/units.md](../../contracts/units.md) | Full unit catalog contract |
| [contracts/game-rules.md](../../contracts/game-rules.md) | Gameplay rules, movement timing, player presence |
| [contracts/resources.md](../../contracts/resources.md) | Terrain resource ids used in recipes and extraction |
| `infinity/src/modules/units/constants/unit-catalog.ts` | Canonical seed data (`UNIT_CATALOG`) |

## Related domains

- [resources/](../resources/) — terrain resources and how units extract or consume them
- [OKF root](../index.md)

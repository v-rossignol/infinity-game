---
title: Buildings
author: Composer
created: 2026-07-08
updated: 2026-07-20
status: draft
tags:
  - units
  - buildings
  - catalog
sources:
  - ../../../contracts/units.md
---

# Buildings

Immobile unit types (`category: building`). Values match `UNIT_CATALOG` in `infinity/src/modules/units/constants/unit-catalog.ts`.

## Summary

| Id | Name | Size | Environments | Recipe |
| -- | ---- | ---- | ------------ | ------ |
| `sawmill` | Sawmill | `small` | `forest` | 25 wood, 100 stone; 100 work |
| `quarry` | Quarry | `small` | `mountain` | 100 wood, 50 stone; 150 work |
| `forge` | Forge | `small` | all land biomes | 150 wood, 250 stone; 200 work |
| `dock` | Dock | `small` | compound `ocean+*` (see [dock.md](dock.md)) | 200 wood, 200 stone; 250 work |
| `mine` | Mine | `small` | `mountain`, `volcanic` | 200 wood, 300 stone; 250 work |

## Catalog entries

| Document | Description |
| -------- | ----------- |
| [sawmill.md](sawmill.md) | Sawmill — forest structure that extracts wood |
| [quarry.md](quarry.md) | Quarry — mountain structure that extracts stone |
| [forge.md](forge.md) | Forge — land structure that transforms ore into ingots |
| [dock.md](dock.md) | Dock — coastal side-zone structure between ocean and land biomes |
| [mine.md](mine.md) | Mine — mountain/volcanic structure that extracts raw ores |

## Related

- [vehicules/](../vehicules/) — mobile unit types
- [../overview.md](../overview.md) — UnitType model and implementation status
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../index.md](../index.md) — units domain overview

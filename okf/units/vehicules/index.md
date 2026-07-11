---
title: Vehicules
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - vehicules
  - catalog
sources:
  - ../../../contracts/units.md
---

# Vehicules

Mobile unit types (`category: vehicule`). Values match `UNIT_CATALOG` in `infinity/src/modules/units/constants/unit-catalog.ts`.

## Summary

| Id | Name | Size | Environments | Move range | Speed |
| -- | ---- | ---- | ------------ | ---------- | ----- |
| `scout-x1` | Scout-X1 | `small` | All planet biomes except `ocean` | 1 hex | 1 |

## Catalog entries

| Document | Description |
| -------- | ----------- |
| [scout-x1.md](scout-x1.md) | Scout-X1 — explore, extract, build small structures |

## Related

- [buildings/](../buildings/) — immobile unit types
- [../overview.md](../overview.md) — UnitType model and implementation status
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../index.md](../index.md) — units domain overview

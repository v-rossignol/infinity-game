---
title: Transformed resources
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - resources
  - transformations
  - crafting
sources:
  - ../../contracts/resources.md
  - ../../documentation/transformations/ideas.md
---

# Transformed resources

**Transformed** resources are products from resource transformations on planetary surfaces. They are not harvested from terrain.

Recipes are draft ideas in [documentation/transformations/ideas.md](../../documentation/transformations/ideas.md). Only outputs whose inputs are **permanent or occasional** terrain resources are listed here. Intermediate ingots and alloys that require other transformed outputs are omitted for now.

Transformation is **planned** — not implemented by Infinity Server.

## Catalog

| Id | Name | Type | Inputs | Suggested quantity | Notes |
| --- | --- | --- | --- | --- | --- |
| `copper-ingot` | Copper ingot | Simple transformation | `copper-ore` (10) + `coal` (3) | 7 | Intermediate step for alloys. |
| `iron-ingot` | Iron ingot | Simple transformation | `iron-ore` (10) + `coal` (5) | 8 | Intermediate step for alloys. |
| `gunpowder` | Gunpowder* | Synthesis | `coal` (5) + `sulfur` (5) + `nitre` | 3 | `nitre` is an occasional desert resource. |

\* Requires at least one **occasional** terrain resource as input.

## Transformation types

| Type | Description |
| ---- | ----------- |
| Simple transformation | One primary input → one output |
| Combination | Multiple inputs → one output |
| Advanced refining | Higher-tier processing, often energy- or fuel-intensive |
| Synthesis | Assembly into usable objects or advanced materials |

## Status

| Topic | Status |
| ----- | ------ |
| Recipe drafts | **Draft** |
| Server transformation pipeline | **Planned** |

## Related

- [permanent-resources.md](permanent-resources.md) — primary inputs for simple transformations
- [occasional-resources.md](occasional-resources.md) — occasional inputs (e.g. `nitre` for gunpowder)
- [terrain-resources.md](terrain-resources.md) — biomes and terrain resource model
- [contracts/resources.md](../../contracts/resources.md) — authoritative catalog (all categories)
- [documentation/transformations/ideas.md](../../documentation/transformations/ideas.md) — full transformation ideas (draft)

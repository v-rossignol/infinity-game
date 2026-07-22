---
title: Unit capabilities
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - capabilities
  - extraction
  - cargo
  - building
  - garage
  - transformation
sources:
  - ../../contracts/units.md
---

# Unit capabilities

Optional blocks on `UnitTypeDefinition.capabilities`. Omitted keys mean the type does not have that capability.

Schema shape: [contracts/schemas/responses/unit-capabilities.json](../../contracts/schemas/responses/unit-capabilities.json) and `packages/shared-types/src/Unit.ts`.

## Capability keys

| Key | Applies to | Description |
| --- | ---------- | ----------- |
| `cargo` | vehicules, buildings | Maximum total cargo quantity across all resource types. |
| `extraction` | vehicules, buildings | Planet-surface harvesting. `types` entries match resource ids from [contracts/resources.md](../../contracts/resources.md); `"*"` allows any resource present in the hex biome. |
| `building` | vehicules (today) | Construct other unit types on the planet surface. Targets split by category (`vehicules`, `buildings`) with allowed `sizes` and unit `ids` (or `"*"`). |
| `garage` | buildings | Park vehicules by size slot count (`small`, `medium`, `large`). Present in seed data; JSON Schema for `garage` is not yet in [unit-capabilities.json](../../contracts/schemas/responses/unit-capabilities.json). |
| `transformation` | vehicules, buildings | Take resources from the cargo of a unit to add another one (see [transformed-resources.md](../resources/transformed-resources.md)). |

## Recipes

Recipe shape: [contracts/schemas/responses/unit-recipe.json](../../contracts/schemas/responses/unit-recipe.json).

Ingredients are consumed from the builder's cargo when a build order starts. Ingredient keys are terrain resource ids.

## Related

- [vehicules/](vehicules/) and [buildings/](buildings/) — capability values per seeded unit type
- [../resources/units-and-resources.md](../resources/units-and-resources.md) — resource ids used in extraction and recipes
- [index.md](index.md) — units domain overview

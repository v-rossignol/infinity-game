# Units

```yaml
date: 2026-07-05
author: Roro LeSage
model: Composer
sources:
  - infinity/src/modules/units/constants/unit-catalog.ts
  - infinity/src/modules/units/unit-catalog.service.ts
  - infinity/src/modules/units/entities/unit-type.entity.ts
  - packages/shared-types/src/Unit.ts
  - contracts/schemas/responses/unit-type.json
  - contracts/schemas/responses/unit-capabilities.json
  - contracts/schemas/responses/unit-recipe.json
  - contracts/game-rules.md
  - contracts/resources.md
```

## Overview

Draft catalog of **`UnitType`** definitions for Infinity. Each entry is an immutable unit template; players own **`UnitInstance`** objects created from these types at runtime.

Canonical seed data lives in `infinity/src/modules/units/constants/unit-catalog.ts` (`UNIT_CATALOG`). On server startup, `UnitCatalogService.ensureCatalog()` upserts each definition into the PostgreSQL `unit_type` table. Runtime reads use the database; the TypeScript constants are the authoring source.

Each unit type has a stable **`id`** (kebab-case) for contracts, constants, and persistence. The **Name** column is the human-readable label. Recipe **ingredients** and extraction **types** reference terrain resource ids from [resources.md](resources.md).

**Categories:** `vehicule` (mobile) and `building` (immobile). **Sizes:** `small`, `medium`, `large`.

---

## Status legend

- **Implemented** — seeded in `UNIT_CATALOG` and enforced by Infinity Server today.
- **Planned** — design intent; not in the catalog seed yet.

| Topic | Implemented | Planned |
| ----- | ----------- | ------- |
| Catalog seed | `scout-x1`, `sawmill`, `quarry` | Additional vehicules and buildings |
| Planet surface | Move, extract, build, cargo, park/unpark | — |
| Place levels | `planet` instances for seeded types | `cube`, `starSystem` vehicules |
| Inter-place travel | — | Cube ↔ system ↔ planet movement |

---

## Summary

### Vehicules

| Id | Name | Size | Environments | Move range | Speed |
| -- | ---- | ---- | ------------ | ---------- | ----- |
| `scout-x1` | Scout-X1 | `small` | All planet biomes except `ocean` | 1 hex | 1 |

### Buildings

| Id | Name | Size | Environments | Recipe |
| -- | ---- | ---- | ------------ | ------ |
| `sawmill` | Sawmill | `small` | `forest` | 25 wood, 100 stone; 100 work |
| `quarry` | Quarry | `small` | `mountain` | 100 wood, 50 stone; 150 work |

---

## Catalog entries

Values match `UNIT_CATALOG` in `infinity/src/modules/units/constants/unit-catalog.ts`.

### `scout-x1` — Scout-X1

**Implemented** · `vehicule` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `true` |
| Speed | `1` (one hex per 2 min at planet surface — see [game-rules.md](game-rules.md) → Vehicule speed) |
| Environments | `plain`, `forest`, `mountain`, `desert`, `ice`, `volcanic` (all `HEX_BIOMES` except `ocean`) |
| Rules | `{ range: hexagon, value: 1 }` — toroidal move-order range |
| Recipe | — (not buildable; granted on first spawn) |

**Capabilities**

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `5`, types `["*"]` |
| `building` | speed `1`, buildings: sizes `["small"]`, units `["*"]` |

Description: *Can explore, extract, and build small structures.*

---

### `sawmill` — Sawmill

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `forest` |
| Rules | — |
| Recipe | `wood` × 25, `stone` × 100; work `100` |

**Capabilities**

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `1`, types `["wood"]` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |

Description: *small forest structure that extracts wood.*

---

### `quarry` — Quarry

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `mountain` |
| Rules | — |
| Recipe | `wood` × 100, `stone` × 50; work `150` |

**Capabilities**

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `1`, types `["stone"]` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |

Description: *small mountain structure that extracts stone.*

---

## Capability keys

Optional blocks on `UnitTypeDefinition.capabilities`. Omitted keys mean the type does not have that capability. Shape: [unit-capabilities.json](schemas/responses/unit-capabilities.json) and `packages/shared-types/src/Unit.ts`.

| Key | Applies to | Description |
| --- | ---------- | ----------- |
| `cargo` | vehicules, buildings | Maximum total cargo quantity across all resource types. |
| `extraction` | vehicules, buildings | Planet-surface harvesting. `types` entries match resource ids from [resources.md](resources.md); `"*"` allows any resource present in the hex biome. |
| `building` | vehicules (today) | Construct other unit types on the planet surface. Targets split by category (`vehicules`, `buildings`) with allowed `sizes` and unit `ids` (or `"*"`). |
| `garage` | buildings | Park vehicules by size slot count (`small`, `medium`, `large`). Present in seed data; JSON Schema for `garage` is not yet in [unit-capabilities.json](schemas/responses/unit-capabilities.json). |

Recipe shape: [unit-recipe.json](schemas/responses/unit-recipe.json). Ingredients are consumed from the builder's cargo when a build order starts.

---

## Related documents

- [game-rules.md](game-rules.md) — gameplay rules, movement timing, player presence
- [resources.md](resources.md) — terrain resource ids used in recipes and extraction
- [Unit model](../documentation/units/units.md) — extended field reference (`UnitType`, `UnitInstance`)
- [Unit instances](../documentation/units/unit-instances.md) — runtime instance storage and API shapes

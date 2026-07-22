# Resources

```yaml
date: 2026-07-11
author: Roro LeSage
model: Composer
sources:
  - okf/resources/index.md
  - okf/resources/permanent-resources.md
  - okf/resources/occasional-resources.md
  - okf/resources/transformed-resources.md
  - packages/shared-config/src/terrain-resources.ts
  - infinity/src/shared/constants/terrain-resources.constants.ts
  - infinity/src/shared/utils/hex-resources.ts
  - infinity/src/modules/resources/resources.service.ts
  - infinity/src/modules/resources/resources.controller.ts
  - contracts/schemas/responses/planet-hex-resources.json
  - contracts/game-api.yaml
  - contracts/game-rules.md
  - documentation/transformations/ideas.md
```

## Overview

Draft catalog of **terrain resources** on planetary hex cells. Canonical constants live in `packages/shared-config/src/terrain-resources.ts` (`PERMANENT_TERRAIN_RESOURCES`, `OCCASIONAL_TERRAIN_RESOURCES`); the server re-exports them from `infinity/src/shared/constants/terrain-resources.constants.ts`.

**Permanent resources** are resolved at request time from the hex biome and returned by `GET /infinity/resources/planet/{planetId}/hex/{q}/{r}`. Planet surface generation stores `resources: []` on every hex; the endpoint does not read persisted hex resources.

**Occasional resources** are defined in `OCCASIONAL_TERRAIN_RESOURCES` alongside permanents. Procedural placement and API exposure are **planned**, not implemented.

Each resource has a stable **`id`** (kebab-case) for contracts, constants, and persistence. The **Name** column is the human-readable label. The hex endpoint maps `id` → `type`, `quantity` → `abundance`, and `rarity` from the shared resource catalog (`common` for permanent terrain resources today).

---

## Terrain resources

Summary matrix of permanent and occasional resources by **terrain** (`HexBiome`).

| Terrain | Permanent resources | Occasional resources |
| ------- | ------------------- | -------------------- |
| `plain` | Food, Fresh water | Rare earths, Uranium |
| `forest` | Wood, Food | Bauxite |
| `mountain` | Stone, Iron ore, Copper ore, Coal | Gold ore, Silver ore |
| `desert` | Silica, Alkaline minerals | Oil, Nitre, Lithium |
| `ocean` | Food, Salt water | Polymetallic nodules, Oil |
| `ice` | Ice, Cryogenic materials | Tritium, Methane ice |
| `volcanic` | Obsidian, Sulfur, Basalt | Industrial diamonds |

---

## Permanent resources

Always present on every hex of matching terrain. One row per terrain–resource pair. Quantities match `PERMANENT_TERRAIN_RESOURCES` in `packages/shared-config/src/terrain-resources.ts`.

| Id | Name | Terrain | Quantity |
| --- | --- | --- | --- |
| `food` | Food | `plain` | 10 |
| `fresh-water` | Fresh water | `plain` | 10 |
| `wood` | Wood | `forest` | 50 |
| `food` | Food | `forest` | 5 |
| `stone` | Stone | `mountain` | 75 |
| `iron-ore` | Iron ore | `mountain` | 10 |
| `copper-ore` | Copper ore | `mountain` | 10 |
| `coal` | Coal | `mountain` | 5 |
| `silica` | Silica | `desert` | 100 |
| `alkaline-minerals` | Alkaline minerals | `desert` | 5 |
| `food` | Food | `ocean` | 30 |
| `salt-water` | Salt water | `ocean` | 100 |
| `ice` | Ice | `ice` | 100 |
| `cryogenic-materials` | Cryogenic materials | `ice` | 5 |
| `obsidian` | Obsidian | `volcanic` | 10 |
| `sulfur` | Sulfur | `volcanic` | 10 |
| `basalt` | Basalt | `volcanic` | 10 |

---

## Occasional resources

**Planned** — present only on some hexagons of matching terrain. Ids and quantities match [okf/resources/occasional-resources.md](../okf/resources/occasional-resources.md). Not yet wired in surface generation or the hex resources endpoint.

| Id | Name | Terrains | Quantity |
| --- | --- | --- | --- |
| `rare-earths` | Rare earths | `plain` | 5 |
| `uranium` | Uranium | `plain` | 2 |
| `bauxite` | Bauxite | `forest` | 7 |
| `gold-ore` | Gold ore | `mountain` | 1 |
| `silver-ore` | Silver ore | `mountain` | 2 |
| `oil` | Oil | `desert`, `ocean` | 10 |
| `nitre` | Nitre | `desert` | 5 |
| `lithium` | Lithium | `desert` | 5 |
| `polymetallic-nodules` | Polymetallic nodules | `ocean` | 10 |
| `tritium` | Tritium | `ice` | 1 |
| `methane-ice` | Methane ice | `ice` | 2 |
| `industrial-diamonds` | Industrial diamonds | `volcanic` | 2 |

Notes:

- **Ids** — stable identifiers for contracts, constants, and persistence; display names stay human-readable in the **Name** column.

---

## Transformed resources (planned)

**Planned** — products from resource transformations; not harvested from terrain. Recipes are draft ideas in `documentation/transformations/ideas.md`. Only outputs whose inputs are **permanent or occasional** terrain resources are listed here (intermediate ingots and alloys that require other transformed outputs are omitted for now).

| Id | Name | Type | Inputs | Suggested quantity | Notes |
| --- | --- | --- | --- | --- | --- |
| `copper-ingot` | Copper ingot | Simple transformation | `copper-ore` (10) + `coal` (3) | 7 | Intermediate step for alloys. |
| `iron-ingot` | Iron ingot | Simple transformation | `iron-ore` (10) + `coal` (5) | 8 | Intermediate step for alloys. |
| `gunpowder` | Gunpowder* | Synthesis | `coal` (5) + `sulfur` (5) + `nitre` | 3 | `nitre` is an occasional desert resource. |

\\* Requires at least one **occasional** terrain resource as input.

---

## Related documents

- [okf/resources/index.md](../okf/resources/index.md) — OKF resources domain (navigation and category docs)
- [Planet](../infinity/documentation/objects/planet.md) — hex surface, biomes, and generation
- [game-rules.md](game-rules.md) — gameplay rules (resources harvesting planned)
- [transformations/ideas.md](../documentation/transformations/ideas.md) — transformation recipes (draft)

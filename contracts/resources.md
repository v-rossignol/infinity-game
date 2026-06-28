# Resources

```yaml
date: 2026-06-20
author: Roro LeSage
model: Composer
sources:
  - infinity/src/shared/constants/terrain-resources.constants.ts
  - infinity/src/shared/utils/hex-resources.ts
  - infinity/src/modules/resources/resources.service.ts
  - infinity/src/modules/resources/resources.controller.ts
  - contracts/schemas/responses/planet-hex-resources.json
  - contracts/game-api.yaml
  - contracts/game-rules.md
```

## Overview

Draft catalog of **terrain resources** on planetary hex cells. **Permanent resources** are implemented in `PERMANENT_TERRAIN_RESOURCES` and returned by `GET /infinity/resources/planet/{planetId}/hex/{q}/{r}`. **Occasional resources** are defined here only — procedural placement is **planned**, not implemented.

Each resource has a stable **`id`** (kebab-case) for contracts, constants, and persistence. The **Name** column is the human-readable label.

---

## Terrain resources

Summary matrix of permanent and occasional resources by **terrain** (`HexBiome`).

| Terrain | Permanent resources | Occasional resources |
| ------- | ------------------- | -------------------- |
| `plain` | Food, Fresh water | Rare earths, Uranium |
| `forest` | Wood, Food | Bauxite deposits |
| `mountain` | Stone, Iron ore, Copper ore, Coal | Gold, Silver |
| `desert` | Silica, Alkaline minerals | Crude oil, Nitre, Brine lithium |
| `ocean` | Food, Salt water | Polymetallic nodules, Oil |
| `ice` | Ice, Cryogenic materials | Tritium, Methane ice |
| `volcanic` | Obsidian, Sulfur, Basalt | Industrial diamonds |

---

## Permanent resources

Always present on every hex of matching terrain. One row per terrain–resource pair. Quantities match `PERMANENT_TERRAIN_RESOURCES` in code.

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

**Planned** — present only on some hexagons of matching terrain. Not yet wired in server generation or constants.

| Id | Name | Terrains | Quantity |
| --- | --- | --- | --- |
| `rare-earths` | Rare earths | `plain` | 100 |
| `uranium` | Uranium | `plain` | 100 |
| `bauxite` | Bauxite deposits | `forest` | 100 |
| `gold` | Gold | `mountain` | 100 |
| `silver` | Silver | `mountain` | 100 |
| `crude-oil` | Crude oil | `desert`, `ocean` | 100 |
| `nitre` | Nitre | `desert` | 100 |
| `brine-lithium` | Brine lithium | `desert` | 100 |
| `polymetallic-nodules` | Polymetallic nodules | `ocean` | 100 |
| `tritium` | Tritium | `ice` | 100 |
| `methane-ice` | Methane ice | `ice` | 100 |
| `industrial-diamonds` | Industrial diamonds | `volcanic` | 100 |

Notes:

- **Crude oil** — listed as *Oil* on `ocean` terrain in the summary matrix; both refer to the same resource (`crude-oil`).
- **Ids** — stable identifiers for contracts, constants, and persistence; display names stay human-readable in the **Name** column.

---

## Related endpoints

| Method | Path | Auth | Behavior |
| ------ | ---- | ---- | -------- |
| GET | `/infinity/resources/planet/{planetId}/hex/{q}/{r}` | Public | Returns permanent terrain resources for the hex at axial coordinates `(q, r)`. |
| GET | `/infinity/resources/planet/{planetId}` | Public | Lists harvestable resource nodes from the `resources` MongoDB collection (may be empty). |

See [game-api.yaml](game-api.yaml) and [planet-hex-resources.json](schemas/responses/planet-hex-resources.json) for response shapes.

---

## Related documents

- [Planet](../infinity/documentation/objects/planet.md) — hex surface, biomes, and generation
- [game-rules.md](game-rules.md) — gameplay rules (resources harvesting planned)

---

## title: Occasional resources

author: Composer
created: 2026-07-08
updated: 2026-07-14
status: draft
tags:

- resources
- terrain
- occasional
sources:
- ../../contracts/resources.md
- ../../packages/shared-config/src/terrain-resources.ts

# Occasional resources

**Occasional** terrain resources are present only on some hexagons of matching terrain. Ids and quantities are defined in `OCCASIONAL_TERRAIN_RESOURCES` in [packages/shared-config/src/terrain-resources.ts](../../packages/shared-config/src/terrain-resources.ts) alongside permanents.

Procedural placement and API exposure are **planned** — not yet wired in surface generation or the hex resources endpoint.

Each resource has a stable `id` (kebab-case) for contracts, constants, and persistence. Display names stay human-readable in the **Name** column.

## Catalog


| Id                     | Name                 | Terrains          | Quantity |
| ---------------------- | -------------------- | ----------------- | -------- |
| `rare-earths`          | Rare earths          | `plain`           | 5        |
| `uranium`              | Uranium              | `plain`           | 2        |
| `bauxite`              | Bauxite              | `forest`          | 7        |
| `gold-ore`             | Gold ore             | `mountain`        | 1        |
| `silver-ore`           | Silver ore           | `mountain`        | 2        |
| `oil`                  | Oil                  | `desert`, `ocean` | 10       |
| `nitre`                | Nitre                | `desert`          | 5        |
| `lithium`              | Lithium              | `desert`          | 5        |
| `polymetallic-nodules` | Polymetallic nodules | `ocean`           | 10       |
| `tritium`              | Tritium              | `ice`             | 1        |
| `methane-ice`          | Methane ice          | `ice`             | 2        |
| `industrial-diamonds`  | Industrial diamonds  | `volcanic`        | 2        |




## Terrain summary


| Terrain    | Occasional resources      |
| ---------- | ------------------------- |
| `plain`    | Rare earths, Uranium      |
| `forest`   | Bauxite                   |
| `mountain` | Gold ore, Silver ore      |
| `desert`   | Oil, Nitre, Lithium       |
| `ocean`    | Polymetallic nodules, Oil |
| `ice`      | Tritium, Methane ice      |
| `volcanic` | Industrial diamonds       |




## Status


| Topic                      | Status      |
| -------------------------- | ----------- |
| Constants in shared-config | **Defined** |
| Procedural hex placement   | **Planned** |
| Hex resources API exposure | **Planned** |




## Related

- [permanent-resources.md](permanent-resources.md) — resources always present on matching terrain
- [transformed-resources.md](transformed-resources.md) — products that may require occasional inputs
- [terrain-resources.md](terrain-resources.md) — biomes and terrain resource model
- [contracts/resources.md](../../contracts/resources.md) — authoritative catalog (all categories)


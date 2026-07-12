---

## title: Building zones
author: Composer
created: 2026-07-11
updated: 2026-07-12
status: approved
tags:
  - places
  - planets
  - hexagons
  - geometry
  - subdivision
  - buildings
sources:
  - ../../../packages/shared-config/src/building-zones.ts
  - ../../../packages/shared-utils/src/game/unit-build-placement.ts
  - ../../../contracts/schemas/players/build-unit.json
  - ../../../contracts/schemas/shared/vec2-local.json
  - ../../../../tools/hex-building-squares/documentation/hex-division-result.md

# Building zones

**Implemented** — logical subdivision of a planet hex into **15 equal-sized squares** for building placement and neighbor-facing interactions. Canonical constants in `@infinity/shared-config` (`building-zones.ts`); placement helpers in `@infinity/shared-utils` (`unit-build-placement.ts`). Used by Terra View build mode and the server build API.

Interactive reference: `[tools/hex-building-squares/](../../../../tools/hex-building-squares/)`.

## Hex reference

Zones sit inside the same normalized pointy-top hex as [hex.md](hex.md) — center `(0.5, 0.5)`, vertices and bounding box defined by `PLANET_HEX_VERTEX_FRACTIONS`. All zone centers and sizes use that `0`–`1` local space (`Vec2Local`).

On the [toroidal hex grid](hex-grid.md), each hex has six surface neighbors; the **6 side zones** name those directions explicitly at the hex edge.

## Square size

Every zone uses the same side length:

```ts
export const HEX_SQUARE_SIZE = 0.175;
```

All **15** zones share this size.

## Central grid

A regular **3 × 3** grid centered on the hex (`HEX_BUILDING_CENTRAL_GRID_SIZE = 3`). Zone **centers**:


| id            | col | row | x     | y     | Rotation |
| ------------- | --- | --- | ----- | ----- | -------- |
| `central-0-0` | 0   | 0   | 0.325 | 0.325 | 0°       |
| `central-1-0` | 1   | 0   | 0.500 | 0.325 | 0°       |
| `central-2-0` | 2   | 0   | 0.675 | 0.325 | 0°       |
| `central-0-1` | 0   | 1   | 0.325 | 0.500 | 0°       |
| `central-1-1` | 1   | 1   | 0.500 | 0.500 | 0°       |
| `central-2-1` | 2   | 1   | 0.675 | 0.500 | 0°       |
| `central-0-2` | 0   | 2   | 0.325 | 0.675 | 0°       |
| `central-1-2` | 1   | 2   | 0.500 | 0.675 | 0°       |
| `central-2-2` | 2   | 2   | 0.675 | 0.675 | 0°       |


The grid spans `0.525 × 0.525`, leaving margin inside the hex outline.

## Side zones

One zone per hex side, centered on the side midpoint. The side passes through the zone center — roughly half inside the hex, half outside.


| id            | Center          | Rotation |
| ------------- | --------------- | -------- |
| `upper-right` | `(0.75, 0.125)` | 29.88°   |
| `right`       | `(1.00, 0.50)`  | 90°      |
| `lower-right` | `(0.75, 0.875)` | −29.88°  |
| `lower-left`  | `(0.25, 0.875)` | 29.88°   |
| `left`        | `(0.00, 0.50)`  | 90°      |
| `upper-left`  | `(0.25, 0.125)` | −29.88°  |


Side rotations match the sloped edges of the pointy-top hex (not a uniform 29.88°).

Since each side square is the interface with exactly one neighboring hexagon, the mapping is simply the opposite side in the neighbor.


| This hex      | Neighbor   | Corresponding square in neighbor |
| ------------- | ---------- | -------------------------------- |
| `upper-left`  | North      | `lower-right`                    |
| `upper-right` | North-East | `lower-left`                     |
| `right`       | South-East | `left`                           |
| `lower-right` | South      | `upper-left`                     |
| `lower-left`  | South-West | `upper-right`                    |
| `left`        | North-West | `right`                          |




## Zone count


| Kind      | Count  | Constant                          |
| --------- | ------ | --------------------------------- |
| Central   | 9      | `HEX_BUILDING_CENTRAL_ZONE_COUNT` |
| Side      | 6      | `HEX_BUILDING_SIDE_ZONE_COUNT`    |
| **Total** | **15** | `HEX_BUILDING_ZONE_COUNT`         |




## Build placement

Clicks snap to the nearest valid **anchor** (`normalizedPointToBuildPlacementAnchor`). The resolved zone id becomes API `buildingZoneId` (`build-unit.json`).

**Footprint size** by unit size (`getBuildFootprintCells`):


| Unit size | Footprint                | Valid anchors                                  |
| --------- | ------------------------ | ---------------------------------------------- |
| `small`   | 1 zone                   | Any central zone, or any side zone             |
| `medium`  | 2 adjacent central zones | Horizontal or vertical pair in the 3 × 3 grid  |
| `large`   | 3 adjacent central zones | Horizontal or vertical strip in the 3 × 3 grid |


**Validity:** every corner of the footprint (including rotation on side zones) must lie inside the hex polygon (`isBuildPlacementValid` / `isPointInPlanetHexLocal`).

## Gameplay roles


| Area                   | Intended use                                                                                                           |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **3 × 3 central grid** | Buildings, terrain features, units inside the hex                                                                      |
| **6 side zones**       | Neighbor directions — movement entry/exit, directional interactions, roads/networks, propagation across hex boundaries |




## Design notes

`HEX_SQUARE_SIZE = 0.175` is intentionally below the theoretical maximum to allow visual spacing, floating-point tolerance, overlay room, and future gameplay additions. The subdivision is a **logical gameplay layout**, not a space-filling tessellation of the hex polygon.

## Related

- [hex.md](hex.md) — hex shape, bounding box, and local coordinates
- [hex-grid.md](hex-grid.md) — toroidal surface tiling and hex-step neighbors
- [../../../../tools/hex-building-squares/documentation/hex-division-result.md](../../../../tools/hex-building-squares/documentation/hex-division-result.md) — original design proposal


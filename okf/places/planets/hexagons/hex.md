---
title: Hex
author: Composer
created: 2026-07-11
updated: 2026-07-12
status: approved
tags:
  - places
  - planets
  - hexagons
  - geometry
sources:
  - ../../../contracts/schemas/responses/planet-hexagon.json
  - ../../../contracts/schemas/shared/vec2-local.json
  - ../../../contracts/schemas/players/build-unit.json
  - ../../../packages/shared-config/src/constants.ts
  - ../../../packages/shared-config/src/building-zones.ts
  - ../../../packages/shared-utils/src/game/unit-build-placement.ts
  - ../../../infinity/src/shared/utils/planet-surface-travel.ts
---

# Hex

A **hex** is one cell on a planet surface — a **pointy-top** regular hexagon drawn inside a fixed-width bounding rectangle.

**Implemented** — same shape in server travel math, Terra View rendering, and build placement.

## Bounding box

Each cell uses a shared layout size (`PLANET_HEX_LAYOUT_WIDTH` × `PLANET_HEX_LAYOUT_HEIGHT`). Positions inside the cell are expressed as **normalized local coordinates** `(x, y)` in the range `0`–`1` relative to that rectangle (`Vec2Local`).

World position = hex grid origin + local offset scaled by cell width/height (`planetSurfaceToWorldPoint`).

## Shape

Six vertices (fractions of cell width/height), clockwise from the top point:

| Vertex | `(x, y)` |
| ------ | -------- |
| Top | `(0.5, 0)` |
| Upper right | `(1, 0.25)` |
| Lower right | `(1, 0.75)` |
| Bottom | `(0.5, 1)` |
| Lower left | `(0, 0.75)` |
| Upper left | `(0, 0.25)` |

The cell **center** is `(0.5, 0.5)` in local space. A point is inside the hex when it lies inside this polygon (`isHexLocalPointInside`).

## Subdivisions

Build placement uses the **15-zone** model in [building-zones.md](building-zones.md) — 9 central squares plus 6 side squares, all `HEX_SQUARE_SIZE` wide in normalized local space. Server and Terra View share placement helpers in `@infinity/shared-utils` (`unit-build-placement.ts`).

## Distance unit

Surface travel uses **hex units**: `1` = the longest vertex-to-vertex distance within a single hex (`getMaxIntraHexDistance`). Crossing that span at speed `1` takes `PLANET_BASE_MOVEMENT_MS_PER_HEX` (2 minutes).

## Data model

A stored surface hex (`PlanetHexagon`) has grid `coordinates` `(q, r)`, a `biome`, and gameplay fields (`resources`, `dangerLevel`). Continuous position on the surface combines hex coordinates with optional in-hex `position` on `PlanetLocation`.

## Related

- [building-zones.md](building-zones.md) — 15-zone build subdivision and placement rules
- [hex-grid.md](hex-grid.md) — how hexes tile into a toroidal surface
- [index.md](index.md) — hexagons subdomain
- [../../../resources/](../../../resources/) — terrain resources by hex biome

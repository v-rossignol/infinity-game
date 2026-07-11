---
title: Hex
author: Composer
created: 2026-07-11
updated: 2026-07-11
status: draft
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

For **build placement**, each hex is overlaid with a square **6 × 6** grid in normalized local space (`PLANET_HEX_BUILD_GRID_DIVISIONS`). Each subdivision cell is `1/6` wide and `1/6` tall (`0`–`1` on both axes).

| Concept | Description |
| ------- | ----------- |
| Grid anchor `(col, row)` | Top-left corner of a unit footprint on the 6 × 6 grid |
| Footprint | Square block of `n × n` grid cells occupied by the built unit |
| `targetPosition` | Normalized center of the footprint sent to the build API (`build-unit.json`) |

**Footprint size** by unit size (`getBuildFootprintCells`):

| Unit size | Footprint (`n × n` cells) |
| --------- | ------------------------- |
| `small` | `1 × 1` |
| `medium` | `2 × 2` |
| `large` | `3 × 3` |

A click or drag inside the hex is snapped to the anchor whose footprint is centered on the pointer (`normalizedPointToBuildGridAnchor`). The anchor is clamped so the footprint stays inside the `6 × 6` bounds (e.g. a `3 × 3` footprint can anchor at `col`/`row` `0`–`3` only).

**Validity:** all four corners of the footprint rectangle must lie inside the hex polygon (`isBuildFootprintInsideHex`). Placements that would stick out past the sloped edges are rejected.

The grid uses **6** divisions so the largest footprint (`3 × 3`) still has at least one cell of margin on each side. Server and Terra View share the same helpers in `@infinity/shared-utils` (`unit-build-placement.ts`).

## Distance unit

Surface travel uses **hex units**: `1` = the longest vertex-to-vertex distance within a single hex (`getMaxIntraHexDistance`). Crossing that span at speed `1` takes `PLANET_BASE_MOVEMENT_MS_PER_HEX` (2 minutes).

## Data model

A stored surface hex (`PlanetHexagon`) has grid `coordinates` `(q, r)`, a `biome`, and gameplay fields (`resources`, `dangerLevel`). Continuous position on the surface combines hex coordinates with optional in-hex `position` on `PlanetLocation`.

## Related

- [hex-grid.md](hex-grid.md) — how hexes tile into a toroidal surface
- [index.md](index.md) — hexagons subdomain
- [../../../resources/](../../../resources/) — terrain resources by hex biome

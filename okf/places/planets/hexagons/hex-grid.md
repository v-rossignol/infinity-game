---
title: Hex grid
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
  - ../../../contracts/game-rules.md
  - ../../../contracts/schemas/shared/hex-coords.json
  - ../../../infinity/src/shared/utils/planet-surface-generation.ts
  - ../../../infinity/src/shared/utils/hex-grid.ts
---

# Hex grid

Planet surfaces use a **toroidal** hex grid — a finite rectangle of axial coordinates that wraps on both axes, tiled as a flat pointy-top honeycomb.

**Implemented** — shared by server generation, movement range, and Terra View.

## Coordinates

Each cell has axial coordinates `(q, r)`:

| Axis | Range | Wraps |
| ---- | ----- | ----- |
| `q` | `0` … `radius − 1` | Yes — left ↔ right |
| `r` | `0` … `radius` | Yes — top ↔ bottom |

Planet `radius` sets grid **width** = `radius` and **height** = `radius + 1`. Cell count: `radius × (radius + 1)`.

## Torus topology

Opposite edges are identified: exiting the right column re-enters on the left, and the top row neighbors the bottom row. The surface has no hard boundary — it is a **torus** laid out as a flat patch.

## Layout geometry

Hexes use **pointy-top** flat tiling:

- Column `q` advances horizontally at fixed pitch.
- Row `r` advances vertically at `0.75 × hexHeight`.
- Odd rows shift horizontally by half a column (`r % 2`) so rows interlock.

World/screen positions follow this layout (`axialToScreen` — server and Terra View). Cell shape and in-hex coordinates: [hex.md](hex.md).

## Neighbors and distance

**Adjacency** follows the **rendered** toroidal layout: the six closest cell centers within adjacency threshold, counting toroidal shifts of the grid (`getNeighbors` in `planet-surface-generation.ts`). This matches what players see on the map.

**Step distance** (movement, range) is the shortest path in that adjacency graph (`hexMoveStepDistance`).

**Coordinate distance** on the torus can also be computed in axial/cube space over wrap copies (`toroidalHexDistance` in `hex-grid.ts`).

## Related

- [index.md](index.md) — hexagons subdomain
- [../planet.md](../planet.md) — planet identity and surface summary
- [../../../contracts/game-rules.md](../../../contracts/game-rules.md) → Places → Planets → Surface topology

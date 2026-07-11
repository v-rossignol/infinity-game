---
title: Places
author: Composer
created: 2026-07-11
updated: 2026-07-11
status: draft
tags:
  - places
  - galaxy
  - planets
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/game-api.yaml
  - ../../contracts/schemas/shared/player-location.json
---

# Places

Knowledge domain for **places** in Infinity — the spatial hierarchy where players move, spawn, and interact.

## Purpose

Organize place-related knowledge from the galaxy down to planetary surface hexes: structure, generation status, and how presence is modeled.

## Hierarchy

| Level | Description |
| ----- | ----------- |
| Galaxy | Open-world container for all places |
| Cube | 10 light-year edge regions |
| Star | Hosts a stellar system; spectral types (`yellow`, `red`, `blue`, `white`) |
| Stellar system | A star and its orbiting planets |
| Planet | Toroidal hex surface with biomes and terrain resources |
| Hexagon | Single surface cell (`q`, `r`) with biome and resources |

Cube, star, stellar system, and planet generation are **implemented**; colonization and territorial gameplay on those places are **planned** (see [contracts/game-rules.md](../../contracts/game-rules.md) → Places).

## Child domains

| Domain | Description |
| ------ | ----------- |
| [planets/](planets/) | Planetary places — surface topology, types, and hex grid |

## Authoritative sources

| Document | Role |
| -------- | ---- |
| [contracts/game-rules.md](../../contracts/game-rules.md) | Places hierarchy, player presence, surface topology |
| [contracts/game-api.yaml](../../contracts/game-api.yaml) | Galaxy, stars, planets REST endpoints |
| [contracts/schemas/shared/player-location.json](../../contracts/schemas/shared/player-location.json) | Player presence at cube, star system, and planet levels |

## Related domains

- [spawn/](../spawn/) — first entry, home planet, spawn selection
- [resources/](../resources/) — terrain resources on planetary hexes
- [units/](../units/) — unit place levels and inter-place travel
- [OKF root](../index.md)

---

## title: Planet
author: Composer
created: 2026-07-11
updated: 2026-07-11
status: draft
tags:
  - places
  - planets
  - surface
sources:
  - ../../../contracts/game-rules.md
  - ../../../contracts/schemas/responses/planet.json
  - ../../../contracts/game-api.yaml

# Planet

A **planet** is a place orbiting a star within a stellar system. It has a procedural **surface** — a toroidal grid of hexagons where players move, spawn, and place units.

**Implemented** — generation and surface API; colonization gameplay is **planned**.

## Identity


| Field          | Role                                                             |
| -------------- | ---------------------------------------------------------------- |
| `_id`          | Planet document id (MongoDB)                                     |
| `name`         | Display name                                                     |
| `starSystemId` | Parent stellar system (UUID)                                     |
| `type`         | Planet class — see below                                         |
| `radius`       | Surface grid size (`5`–`15`); defines column count and row count |




## Planet types


| Type    | Notes                                                              |
| ------- | ------------------------------------------------------------------ |
| `rocky` | Default for admin generation; used for first-entry spawn selection |
| `gas`   | Listed in system summaries                                         |
| `ice`   |                                                                    |
| `lava`  |                                                                    |




## Surface

- **Topology:** toroidal hex grid — see [hexagons/hex-grid.md](hexagons/hex-grid.md).
- **Cells:** each hex has a biome (`desert`, `forest`, `ocean`, `mountain`, `ice`, `volcanic`, `plain`) — see [hexagons/](hexagons/).
- **Resources:** terrain resources are resolved per hex at request time from biome; see [resources/](../../resources/).

Player presence on a planet is modeled as `PlayerLocationOnPlanet` (`starSystemId`, `planetId`, optional hex `q`/`r`).

## Related

- [index.md](index.md) — planets subdomain
- [hexagons/](hexagons/) — surface hex cells
- [../../spawn/home-planet.md](../../spawn/home-planet.md) — home planet identity on first entry
- [../../../contracts/game-rules.md](../../../contracts/game-rules.md) → Places → Planets


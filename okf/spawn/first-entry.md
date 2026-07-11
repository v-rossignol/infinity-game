---
title: First entry
author: Composer
created: 2026-07-09
updated: 2026-07-09
status: draft
tags:
  - spawn
  - first-entry
  - players
  - api
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/game-api.yaml
  - ../../contracts/asyncapi.yaml
  - ../../contracts/schemas/responses/player.json
---

# First entry

**First entry** is when an authenticated user gains their first place in the galaxy. Before that moment the player profile exists but has **no location** (`location: null`).

## Preconditions

| State | Meaning |
| ----- | ------- |
| Authenticated | Valid JWT or session cookie (see [contracts/game-api.yaml](../../contracts/game-api.yaml)) |
| No place yet | `Player.location` is `null` — the player has never completed spawn |

## API — `POST /players/me/enter-game`

Canonical REST entry point (`operationId: enterGame`).

| Aspect | Rule |
| ------ | ---- |
| First call | Orchestrates world spawn: cube → star → planet → hex |
| Later calls | **Idempotent** — returns the existing profile when `location` is already set |
| Success | `200` — player profile (`schemas/responses/player-wrapper.json`) |
| Auth failure | `401` |
| Spawn failure | `503` — spawn allocation failed after retries |

Clients use this as the second bootstrap step after auth (see [contracts/client-terra-view.yaml](../../contracts/client-terra-view.yaml) and [contracts/client-solaris.yaml](../../contracts/client-solaris.yaml)).

## Spawn selection (implemented)

On first entry the server assigns a **home planet** and a surface hex. Selection order from [contracts/game-rules.md](../../contracts/game-rules.md):

1. Choose a **region** of the galaxy (cube).
2. Choose a **star** in that region.
3. Choose the **largest rocky** planet in that stellar system (`type: rocky`).
4. Choose a **random** surface hex on that planet.

After spawn, the player may move through the galaxy according to **current presence** and **view access** rules.

## Planet hex (REST + Socket.IO)

First spawn always assigns a hex on the home planet today.

| Path | Behavior |
| ---- | -------- |
| REST `enter-game` | Sets `location` including `planet` and `hex_coords` |
| Socket `PLANET_JOIN` | Joins the planet room; restores hex from PostgreSQL or rolls a random spawn hex; emits `PLANET_UPDATE` on success |

`PLANET_JOIN` is defined in [contracts/asyncapi.yaml](../../contracts/asyncapi.yaml). Direct instance creation on `enter-game` (starter vehicule) is a separate server path from the unit build system.

## Status

| Topic | Implemented | Planned |
| ----- | ----------- | ------- |
| `enter-game` spawn orchestration | Yes | — |
| Largest-rocky + random-hex selection | Yes | — |
| Idempotent resume | Yes | — |
| Spawn allocation retries / `503` | Yes | — |

## Related

- [home-planet.md](home-planet.md) — permanent home planet identity vs current presence
- [starter-grants.md](starter-grants.md) — units created at spawn
- [index.md](index.md) — spawn domain overview

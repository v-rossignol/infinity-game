---
title: Home planet
author: Composer
created: 2026-07-09
updated: 2026-07-09
status: draft
tags:
  - spawn
  - home-planet
  - players
  - presence
  - planet-surface
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/schemas/responses/player.json
  - ../../contracts/game-api.yaml
  - ../../contracts/resources.md
  - ../../infinity/src/modules/planets/planets.service.ts
  - ../../infinity/src/shared/utils/spawn-planet-surface.ts
---

# Home planet

The **home planet** is where the player **spawns** on first entry. It is a gameplay identity distinct from **current presence** — where the player is active right now.

## Definition

From the [contracts/game-rules.md](../../contracts/game-rules.md) glossary:

- Assigned on **first entry** during spawn selection (largest rocky planet in the chosen stellar system).
- Does **not change** for as long as the player exists.
- Is lost only when the player **disappears** (player removal is not available in the game yet).

## Home planet vs current presence

| Concept | Role | Stored today |
| ------- | ---- | ------------ |
| **Home planet** | Permanent spawn identity; anchor for freshy rules (planned) | Concept in game rules — **not** a separate field on `Player` |
| **Current presence** | Active view depth and position (`Player.location`) | `location` on the player profile (`schemas/responses/player.json`) |

After spawn, current presence starts on the home planet but may **differ** once the player transitions through cube, stellar system, or other planets.

The player schema exposes `location` (or `null` before first entry). It does not yet expose a dedicated `homePlanet` property.

## Spawn planet materialization

On first entry, the server does **not** reuse the generic lazy planet path (`getPlanet` → `createPlanetFromSummary`). It calls **`createSpawnPlanetFromSummary`** in `PlanetsService`, which:

1. Materializes the planet document from the star-system summary (procedural surface generation).
2. Applies **spawn-only surface adjustments** via `applySpawnPlanetSurfaceAdjustments`.
3. Persists the surface when biomes or hex resources changed.

Regular visits to other planets still use `getPlanet` and do **not** receive these adjustments.

## Spawn surface adjustments (implemented)

Applied in order after procedural generation.

### Biome coverage

Every hex surface biome (`desert`, `forest`, `ocean`, `mountain`, `ice`, `volcanic`, `plain`) must appear at least once on the home planet.

| Step | Rule |
| ---- | ---- |
| Detect missing biomes | Compare generated surface against `HEX_BIOMES` |
| For each missing biome | Replace **one** hex whose biome is currently the **most represented** on the surface |
| Tie-break | When counts are equal, prefer the biome that appears first in `HEX_BIOMES` order |
| Resources on replaced hex | Refreshed from permanent terrain rules for the new biome (`resolveHexResources`) |
| `dangerLevel` | Unchanged on the replaced hex |

### Occasional oil guarantee

If the planet has **no** occasional **oil** on any hex after biome coverage:

| Step | Rule |
| ---- | ---- |
| Candidate hexes | Random choice among hexes with biome `ocean` or `desert` |
| Resource added | `oil` — quantity `10`, rarity `occasional` (from `OCCASIONAL_TERRAIN_RESOURCES`) |
| Existing hex resources | Preserved; oil is **appended** to the chosen hex |
| Skip | When oil is already present anywhere on the surface, or when no ocean/desert hex exists |

Biome coverage runs **before** the oil check so ocean and desert hexes are available when the procedural surface omitted them.

## View access (interim)

Terra View access today is **location-based**, not home-planet-based:

| Client | Rule (regular players) |
| ------ | ---------------------- |
| Terra View | `can-enter/planet/{planetId}` is `true` when `location.planet.id` matches the target planet |

Solaris access requires a **star-system** unit (`placeLevel = starSystem`), not merely spawning on a planet. See [starter-grants.md](starter-grants.md).

## Freshy and planned presence rules

A **freshy** owns **no buildings**. Planned rules (not fully enforced today):

- A freshy may only be **present** on their home planet until they own units or buildings elsewhere.
- Non-freshy presence will be limited to places where the player owns units or buildings.

Home planet as a **remembered permanent identity separate from current presence** is marked **planned** in the game-rules status table.

## Status

| Topic | Implemented | Planned |
| ----- | ----------- | ------- |
| Spawn assigns a planet on first entry | Yes | — |
| Spawn-only planet path (`createSpawnPlanetFromSummary`) | Yes | — |
| Full biome coverage on home planet surface | Yes | — |
| Guaranteed occasional oil on home planet | Yes | — |
| Dedicated `homePlanet` field on player profile | — | Yes |
| Freshy presence limited to home planet | — | Yes |
| View access tied to home planet | — | Extend beyond interim location checks |

## Related

- [first-entry.md](first-entry.md) — spawn selection and `enter-game`
- [starter-grants.md](starter-grants.md) — starter vehicule on the home planet
- [../units/overview.md](../units/overview.md) — unit place levels
- [../resources/](../resources/) — permanent and occasional terrain resources
- [index.md](index.md) — spawn domain overview

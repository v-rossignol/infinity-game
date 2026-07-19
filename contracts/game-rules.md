# Game Rules

```yaml
author: Roro LeSage
date: 2026-06-14
modified:
  - date: 2026-06-14
    author: Composer
    model: composer-2.5-fast
  - date: 2026-06-20
    author: Composer
    model: composer-2.5-fast
  - date: 2026-06-21
    author: Roro LeSage
  - date: 2026-06-27
    author: Composer
    model: claude-sonnet-4-5
  - date: 2026-06-27
    author: Composer
    model: composer-2.5-fast
  - date: 2026-06-27
    author: Composer
    model: composer-2.5-fast
  - date: 2026-06-27
    author: Composer
    model: composer-2.5-fast
  - date: 2026-07-14
    author: Composer
    model: composer-2.5-fast
sources:
  - documentation/infinity.md
  - infinity/src/shared/constants/galaxy.constants.ts
  - infinity/src/shared/constants/game.constants.ts
  - infinity/src/shared/utils/planet-surface-travel.ts
  - infinity/src/shared/utils/planet-surface-generation.ts
  - infinity/src/shared/utils/hex-grid.ts
  - infinity/src/shared/interfaces/player-location.interface.ts
  - infinity/src/modules/players/player-location.service.ts
  - infinity/src/modules/units/unit-instance.service.ts
  - infinity/documentation/objects/cube.md
  - infinity/documentation/objects/star.md
  - infinity/documentation/objects/star-system.md
  - infinity/documentation/objects/planet.md
  - contracts/game-api.yaml
  - contracts/asyncapi.yaml
  - contracts/schemas/shared/player-location.json
  - contracts/resources.md
  - contracts/units.md
  - infinity/src/modules/units/unit-building.service.ts
```

Draft business rules for Infinity gameplay, locations, and client/server authority. Rules may be completed or changed.

**AI MUST ask before changing current file**

## Status legend

- **Implemented** — enforced by Infinity Server today.
- **Planned** — design intent; not enforced in code yet.

| Topic | Implemented | Planned |
|-------|-------------|---------|
| Galaxy places | Procedural cubes, stars, systems, planets | Colonization gameplay on those places |
| Player presence | Current presence for transitions; Solaris view access by star-system units | Full unit/building ownership at all depths |
| Freshy | Owns no buildings | Presence limited to home planet until expansion |
| Home planet | Planet where the player spawns | Remembered as a permanent identity separate from current presence |
| First entry | Spawn on the largest rocky planet at a random surface hex | — |
| Vehicules and buildings | Planet surface move, extract, build, cargo, park/unpark; building zone placement | Inter-place command layer; transformation orders |

# Game Vision

Infinity is a multiplayer open-world game set in a dynamically generated galaxy. Players explore, colonize, and interact in real-time across procedurally generated star systems and planets — harvesting resources, crafting, and competing or collaborating with others. Most of this gameplay is **planned**; see the status legend above.

# Places

The game takes place in a galaxy. Cube, star, stellar system, and planet generation are **implemented**; colonization and territorial gameplay on those places are **planned**.

## Cubes

The galaxy is divided into cubes (10 LY for each edge).

## Stars

Stars are defined in cubes. Each star has a stellar system.

### Star types (Implemented)

Each star has a spectral `type` in `properties`: `yellow`, `red`, `blue`, or `white`.

## Stellar Systems

A stellar system describes each planet orbiting its star.

## Planets

A planet is associated with a star. It has a size and surface definition (grid of hexagons) with available resources.

### Surface topology (Implemented)

Planet surfaces use a **toroidal** hex grid: `radius` columns (`q` = 0 … radius − 1) and `radius + 1` rows (`r` = 0 … radius). Both axes wrap — the top and bottom rows are neighbors, as are the left and right columns. Neighbor lookup, move range, and travel distance all use this wrapping (same layout as Terra View and `getNeighbors` in `planet-surface-generation.ts`).

### Planet types (Implemented)

Each planet in a stellar system summary has a `type`: `rocky`, `gas`, `ice`, or `lava`.

## Hexagons

planet is defined as a gid of hexagons. each hexagon has one of the following biomes: desert, forest, ocean, mountain, ice, volcanic, plain

### Resources

Permanent and occasional **terrain resources** by hex biome — stable `id` values, quantities, and implementation status: [resources.md](resources.md).

# Players

There are no team concepts, just players that can interact with game entities and other players.

## Implemented

### First entry

- Before first entry, the player has **no place** in the galaxy.
- On first entry, the player **spawns** on a newly assigned **home planet** (see **Glossary**).
- Spawn selection: a region of the galaxy is chosen, then a star, then the **largest rocky** planet in that stellar system, then a **random** surface hex on that planet.
- After spawn, the player may move through the galaxy according to **current presence** and **view access** rules below.

### While in the game

- The player has a **current presence** — where they are active now in the galaxy (see **Glossary**).
- The server validates moves within the current place and transitions between cube, stellar system, and planet.
- **View access** (which client the player may open) is checked separately via `GET /infinity/players/me/can-enter/*` (see **Movements and Locations → Player presence**).

## Planned

- **Infinity is a god game**: players command vehicules and build buildings.
- A **freshy** may only be **present** on their home planet until they own units or buildings elsewhere.
- Players who are not freshies may only be present where they own units or buildings — in a cube, in a stellar system, or on a planet surface. *(Target model; only Solaris view access is partially enforced today — see **Player presence**.)*

# Units

Players own units.
There are 2 type of units: vehicules and buildings.

Unit instances are stored at a **place level**: `cube`, `starSystem`, or `planet` (see `placeLevel` on unit instances). First spawn creates a planet-level vehicule on the home planet; it does not grant Solaris view access by itself.

## Buildings

Buildings are immobile. They occupy a **building zone** on a planet hex surface.

### Building construction (Implemented)

Planet-surface builds use `POST /infinity/players/me/units/{unitId}/build` with a target hex and `buildingZoneId`.

**Zones** — 15 equal squares per hex (9 central, 6 side). Side zones straddle the hex edge and share a physical slot with the opposite side zone on the neighboring hex.

**Occupancy** — one building (or one in-progress build) per zone. Side-zone checks include the paired hex and opposite side zone id.

**Environments** — see [units.md](units.md) → Environments. Atomic biomes require every involved hex to match. Compound entries (`biomeA+biomeB`) require a side zone with exactly those two biomes on the two hexes.

**Finished instance** — `location.planet` includes `hex_coords`, normalized `position` (zone center), and `buildingZoneId` for building unit types.

Only **small** footprints (one zone) are supported today.

## Vehicules

Vehicules are mobile within their place level (planet surface today).

# Movements and Locations

## Player presence

Two related concepts apply today: **current presence** (stored player `location`) and **view access** (whether a client depth may be opened). They will converge once the full unit-ownership model is implemented.

### Implemented

#### Current presence (transitions and position)

- Governed by stored **current presence** (see **Glossary**).
- Location transitions follow a depth chain: cube → stellar system → planet (and reverse for leave operations).
- Admin users may bypass transition constraints via `adminBypass`.

#### View access (`can-enter` endpoints)

Clients use `GET /infinity/players/me/can-enter/*` before linking to Galaxy View, Solaris, or Terra View. Admin users always receive `canEnter: true`.

| Client depth | Endpoint | Rule (regular players) |
|--------------|----------|------------------------|
| Stellar system (Solaris) | `can-enter/system/{starSystemId}` | `true` only when the player owns at least one unit at **star-system depth** (`placeLevel = starSystem`) in that system. Planet-surface units do **not** grant access. |
| Planet (Terra View) | `can-enter/planet/{planetId}` | `true` only when `location.planet.id` matches the target planet *(interim — location-based)*. |
| Cube (Galaxy View) | `can-enter/cube/{cubeId}` | `true` only when `location.cube.id` matches the target cube *(interim — location-based)*. |

### Planned

- Extend **view access** for cube and planet depths to unit/building ownership (same spirit as Solaris).
- Replace interim location-based `can-enter` checks with ownership at the matching place level.
- Govern **current presence** and transitions by unit/building ownership (see **Players → Planned**).
- **Freshy** restriction: presence limited to home planet until the player owns units or buildings elsewhere.

## Building places

Buildings do not move. They occupy a fixed building zone at planet depth (see **Units → Building construction**).

## Vehicule movement (Planned — inter-place)

Vehicule can move within the place they occupy (cube, stellar system, planet's hexagone).
They may gain the capability to move between places (cube to stellar system, stellar system to planet, and so on).

### Vehicule speed (Implemented — planet surface)

- `speed` is a dimensionless **rate multiplier**.
- `speed: 1` = one hex per 2 minutes at planet-surface depth, calibrated to the largest internal distance of a single hex (vertex to opposite vertex).
- **Travel distance** is computed in continuous surface space from the unit's current `(hex_coords, position)` to the target `(hex_coords, position)`, folding toroidal wraps when the planet `radius` is known. Positions are normalized fractions of the hex cell (0–1 on each axis), using the same pointy-top odd-r layout as Terra View (`PLANET_HEX_LAYOUT_WIDTH` / `PLANET_HEX_LAYOUT_HEIGHT` in `game.constants.ts`). The result is expressed in **hex units**, where `1.0` equals that largest intra-hex distance.
- **Travel time formula:** `travelMs = (surfaceDistanceHexUnits / speed) × PLANET_BASE_MOVEMENT_MS_PER_HEX`, where `PLANET_BASE_MOVEMENT_MS_PER_HEX = 120 000 ms`.
- **Move range** (how far a unit may order a move) uses integer **toroidal** `hexDistance` on the planet grid (`q` wraps by `radius`, `r` wraps by `radius + 1`) from the unit type's `rules` (`range: hexagon`); only timing is position-aware.
- When travel completes, the server pushes `UNIT_UPDATE` on the planet Socket.IO room at the computed `arrivalAt`.
- `speed: null` means the unit type does not move (applies to buildings).
- Higher `speed` values are proportionally faster: `speed: 2` halves travel time.

# Client / Server Authority

The server is authoritative.

The client:

* Sends player intentions.
* Displays game state.
* May perform visual prediction.

The server:

* Validates all actions.
* Applies game rules.
* Maintains the authoritative game state.
* Broadcasts defined events to clients.

# Network Synchronization

* The server is the source of truth.
* Clients may perform visual prediction.
* Server corrections must be applied immediately.
* Entity identifiers are generated by the server.
* Clients must never create authoritative entities.

# AI Development Guidelines

- Never bypass server validation.
- Never modify authoritative state on the client.
- Prefer existing contracts and events before introducing new ones.
- Any change to exposed server behavior must be reflected in the project API contracts.
- Any new game rule must be documented in this file.

# Glossary

Domain terms used in this document. Prefer these definitions over repeated prose elsewhere.

## Home Planet

The planet where the player **spawns** on first entry. It does not change for as long as the player exists and is lost only when the player disappears.

## Freshy

A player who owns **no buildings**.

## Current Presence

The cube, stellar system, or planet where the player is active now (`Player.location`). This may differ from home planet once the player moves through the galaxy. Drives location transitions and interim planet/cube view access.

## View Access

Whether a player may open a given client depth (Galaxy View, Solaris, Terra View). Checked via `GET /infinity/players/me/can-enter/*`. Partially governed by unit ownership today (Solaris only); cube and planet checks are interim and still match current presence.

## Disappears (player)

A player **disappears** when they are no longer present in stored game data. Player removal is not yet available in the game.

## Authoritative State

The official game state maintained by the server.

## Prediction

Temporary client-side simulation used to improve responsiveness.

## Reconciliation

The process of correcting client state using authoritative server updates.

## Interest Management

The system determining which entities are relevant to each client. Room-based filtering exists today; full entity-level interest management is planned.

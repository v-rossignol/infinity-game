# Units

```yaml
date: 2026-06-21
author: Roro LeSage
model: Composer
sources:
  - contracts/game-rules.md
  - documentation/location.md
  - documentation/units/units.md
modified:
  - date: 2026-06-21
    author: Roro LeSage
    note: Align location field with unified Location type (unit profile)
  - date: 2026-06-21
    author: Roro LeSage
    note: Rename biome to environments (place levels and planet biomes)
  - date: 2026-06-21
    author: Roro LeSage
    note: Define rules as UnitRule[] (type pending)
  - date: 2026-06-21
    author: Roro LeSage
    note: Split attributes into UnitType (immutable) and UnitInstance (mutable)
  - date: 2026-06-21
    author: Roro LeSage
    note: Add ownerId to UnitInstance
  - date: 2026-06-21
    author: Roro LeSage
    note: Add capabilities field to UnitType
  - date: 2026-06-21
    author: Roro LeSage
    note: Define UnitRule (range, value)
```

## Overview

Draft specification for **units**, **vehicules**, and **buildings** in Infinity. This behavior is **planned** — not enforced by Infinity Server today. See [game-rules.md](../../contracts/game-rules.md) for related player-presence and movement rules.

A **unit** is a placeable game entity owned by a player. Units specialize into **vehicules** (mobile) and **buildings** (immobile).

Unit data splits into two layers:

| Layer | Type | Role |
| ----- | ---- | ---- |
| **Unit type** | `UnitType` | Immutable definition shared by all instances of the same unit (name, size, rules, etc.). |
| **Unit instance** | `UnitInstance` | Mutable runtime state for one placed unit (location, status, etc.). References a unit type via `typeId`. |

---

## Categories

| Category | `mobility` | Description |
| -------- | ---------- | ----------- |
| `vehicule` | `true` | A unit that can move within its current place. |
| `building` | `false` | A unit that remains fixed at its placement location. |

Set on the unit type as the `type` field.

---

## Unit type

Immutable attributes. One **unit type** defines the template for many **unit instances**.

### Fields

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `id` | string | yes | Unique identifier for the unit type. |
| `name` | string | yes | Display name of the unit type. |
| `type` | enum | yes | Category: `vehicule` or `building`. |
| `size` | enum | yes | Physical size for collisions and range calculations. See [`size`](#size). |
| `mobility` | boolean | yes | Whether instances can move. `true` for vehicules; `false` for buildings. |
| `environments` | string[] | no | Where instances may be placed. Place levels and/or planet biomes. See [`environments`](#environments). |
| `rules` | `UnitRule[]` | no | Gameplay rules or effects. See [`rules`](#rules). Default: `[]`. |
| `capabilities` | object | no | Named capability configs. See [`capabilities`](#capabilities). Default: `{}`. |
| `description` | string | no | Textual description of the unit type’s role or characteristics. |
| `metadata` | object | no | Additional unstructured data for future extensibility. |

### `size`

Physical footprint of the unit type. Used for collisions and range calculations.

| Value | Description |
| ----- | ----------- |
| `small` | Compact unit. |
| `medium` | Standard unit. |
| `large` | Large unit. |

### `environments`

Optional constraints on where instances of this unit type may be placed. Values name either a **place level** (cube, stellar system) or a **planet biome** (hex surface).

| Category | Values | Applies when |
| -------- | ------ | ------------ |
| Place | `cube` | Instance is placed in cube space. |
| Place | `stellar_system` | Instance is placed in a stellar system. |
| Planet biome | e.g. `urban`, `forest`, `desert` | Instance is placed on a planet hex with that biome. |

A unit type may list several values. Every catalog type **must** declare at least one token; empty or omitted `environments` is invalid at deploy time (see [unit-instances.md](unit-instances.md) → Create validation).

**Cube only**

```json
["cube"]
```

**Stellar system only**

```json
["stellar_system"]
```

**Planet biomes**

```json
["forest", "desert"]
```

**Mixed**

```json
["cube", "stellar_system"]
```

### `rules`

Array of **`UnitRule`** objects on the unit type. Each rule describes a scoped gameplay effect on the planet surface.

#### `UnitRule`

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `range` | enum | yes | Unit of scope for the rule. Values: `hexagon`. |
| `value` | number | yes | Magnitude of the rule, counted in `range` units. |

**Example**

```json
[
  {
    "range": "hexagon",
    "value": 1
  }
]
```

### `capabilities`

Map of predefined capability keys to capability-specific objects. Only listed keys apply; omitted keys mean the unit type does not have that capability.

| Key | Object shape | Description |
| --- | ------------ | ----------- |
| `cargo` | `{ "size": number }` | Carrying capacity for resources or goods. |
| `extraction` | `{ "speed": number, "types": string[] }` | Harvest rate and allowed resource types. `types` entries match resource `id` values from [resources.md](../../contracts/resources.md). Use `["*"]` to allow **all** catalog resources. |
| `build` | **to be defined** | Construction capability. |
| `storage` | **to be defined** | Static storage capacity (buildings). |

**Example**

```json
{
  "cargo": { "size": 1000 },
  "extraction": {
    "speed": 10,
    "types": ["iron-ore", "copper-ore"]
  }
}
```

### Vehicule types

A vehicule unit type has `mobility: true` and `type: vehicule`.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `speed` | number | yes | Movement speed within the instance’s current place. Units and scale **to be defined**. |

### Building types

A building unit type has `mobility: false` and `type: building`. Instances cannot move after placement.

### Example

```json
{
  "id": "661e8400-e29b-41d4-a716-446655440001",
  "name": "Transport vehicule",
  "type": "vehicule",
  "size": "medium",
  "mobility": true,
  "environments": ["forest", "desert"],
  "rules": [
    {
      "range": "hexagon",
      "value": 1
    }
  ],
  "capabilities": {
    "cargo": { "size": 1000 },
    "extraction": {
      "speed": 10,
      "types": ["iron-ore", "copper-ore"]
    }
  },
  "description": "Ground transport for resource hauls.",
  "speed": 1,
  "metadata": {
    "faction": "Haqqislam",
    "cost": 20
  }
}
```

---

## Unit instance

Mutable attributes. One **unit instance** is a single placed unit in the galaxy.

Storage, queries, API list shape, and PostgreSQL denormalized columns: [unit-instances.md](unit-instances.md).

### Fields

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `id` | string | yes | Unique identifier for the unit instance. |
| `typeId` | string | yes | Reference to the [`UnitType`](#unit-type) `id`. |
| `ownerId` | string | yes | Player who owns this instance. References [Player](../../contracts/schemas/responses/player.json) `id`. |
| `location` | `Location` | yes | Placement in the galaxy. Same shape as player location; validated with the **`unit`** profile. See [`location`](#location). |
| `status` | enum | yes | Operational state. Default: `inactive`. See [`status`](#status). |
| `createdAt` | date-time | yes | ISO 8601 creation time. |
| `updatedAt` | date-time | yes | ISO 8601 last mutation (create, move, deploy, destroy, etc.). |
| `metadata` | object | no | Additional unstructured runtime data for future extensibility. |

### `status`

Lifecycle state of the instance. Default on creation: **`inactive`**.

| Value | Description |
| ----- | ----------- |
| `inactive` | Placed and idle; no operation in progress. Default after creation or when an operation completes. |
| `active` | Currently executing an operation (moving, building, etc.). |
| `destroyed` | No longer functional. Remains at its last location; cannot act, move, or be reactivated. Removal rules **to be defined**. |

Transitions: `inactive` → `active` (operation starts) → `inactive` (operation completes); any state → `destroyed` when applicable.

### `location`

Instances use the shared **`Location`** type (`UnitLocation` is a type alias). Validation profile: **`unit`**.

| Place | Required fields |
| ----- | --------------- |
| Cube | `cube.id`, `cube.position` (`Vec3Local`) |
| Stellar system | `cube.id`, `starSystem.id`, `starSystem.position` (`Vec2Local`) |
| Planet surface | `cube.id`, `starSystem.id`, `planet.id`, `planet.hex_coords`, `planet.position` (`Vec2Local` within the hex) |

Instances **cannot** be on a planet without a hex. **`planet.position`** is required whenever `hex_coords` is present. Full rules: [location.md](../location.md) → Validation profiles → `unit`.

**Cube**

```json
{
  "cube": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "position": { "x": 3.5, "y": 7.2, "z": 1.0 }
  }
}
```

**Stellar system**

```json
{
  "cube": { "id": "550e8400-e29b-41d4-a716-446655440000" },
  "starSystem": {
    "id": "6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    "position": { "x": 120.5, "y": 340.0 }
  }
}
```

**Planet + hex + in-hex position** *(typical surface placement)*

```json
{
  "cube": { "id": "550e8400-e29b-41d4-a716-446655440000" },
  "starSystem": { "id": "6ba7b810-9dad-11d1-80b4-00c04fd430c8" },
  "planet": {
    "id": "6ba7b811-9dad-11d1-80b4-00c04fd430c8",
    "hex_coords": { "q": 12, "r": 7 },
    "position": { "x": 0.35, "y": 0.72 }
  }
}
```

### Example

```json
{
  "id": "662e8400-e29b-41d4-a716-446655440002",
  "typeId": "scout-x1",
  "ownerId": "773e8400-e29b-41d4-a716-446655440003",
  "location": {
    "cube": { "id": "550e8400-e29b-41d4-a716-446655440000" },
    "starSystem": { "id": "6ba7b810-9dad-11d1-80b4-00c04fd430c8" },
    "planet": {
      "id": "6ba7b811-9dad-11d1-80b4-00c04fd430c8",
      "hex_coords": { "q": 12, "r": 7 },
      "position": { "x": 0.35, "y": 0.72 }
    }
  },
  "status": "inactive",
  "createdAt": "2026-06-21T10:00:00Z",
  "updatedAt": "2026-06-21T10:00:00Z",
  "metadata": {}
}
```

---

## Related documents

- [unit-instances.md](unit-instances.md) — PostgreSQL storage, queries, API (`UnitInstanceWithType`)
- [catalog.md](catalog.md) — unit type catalog (Scout-X1 and future entries)
- [location.md](../location.md) — unified `Location` type and validation profiles
- [game-rules.md](../../contracts/game-rules.md) — player presence, unit movement, and freshy rules
- [resources.md](../../contracts/resources.md) — terrain and harvestable resources
- [planet.md](../../infinity/documentation/objects/planet.md) — planetary surface placement context

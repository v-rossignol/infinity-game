# Unit instances

```yaml
date: 2026-06-21
author: Roro LeSage
model: Composer
sources:
  - documentation/units/units.md
  - documentation/location.md
  - contracts/game-rules.md
  - contracts/resources.md
  - contracts/schemas/responses/player.json
modified:
  - date: 2026-06-21
    author: Roro LeSage
    note: REST paths, FK name, environments mapping, lifecycle, auth, pagination, v1 scope
```

## Overview

**Unit instances** are placed units in the galaxy (`UnitInstance`). Each instance references a catalog entry (`UnitType`) by string id (e.g. `scout-x1`).

This document covers **PostgreSQL storage** (denormalized columns for fast listing), **query rules**, **create validation**, **lifecycle**, and **REST list responses** (instance + embedded type).

Field definitions for `UnitInstance` and `UnitType`: [units.md](units.md). Catalog entries: [catalog.md](catalog.md).

**Status:** planned — not enforced by Infinity Server today.

---

## Identity

| Concept | Detail |
| ------- | ------ |
| Primary key | Instance `id` (UUID) |
| Type reference | `typeId` → [unit catalog](catalog.md) string id (e.g. `scout-x1`), not a UUID |
| Owner | `ownerId` → [Player](../../contracts/schemas/responses/player.json) `id` (UUID) |
| Canonical placement | `location` JSONB (`Location`, **`unit`** validation profile) |

---

## API model

List and detail responses use **Option B**: each item is the **instance** plus an embedded **`type`** object (resolved `UnitType` from the catalog).

Clients do not need a separate catalog fetch to render name, speed, capabilities, or sprites.

### `UnitInstanceWithType`

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | string (UUID) | Instance id |
| `typeId` | string | Catalog type id (duplicate of `type.id` for convenience) |
| `ownerId` | string (UUID) | Owning player |
| `location` | `Location` | Canonical placement |
| `status` | enum | `inactive` \| `active` \| `destroyed` — see [units.md](units.md). **Not used in list filters for v1.** |
| `createdAt` | date-time | Creation time |
| `updatedAt` | date-time | Last mutation (create, move, deploy, destroy, etc.) |
| `metadata` | object | Optional runtime extension |
| `type` | `UnitType` | Full catalog definition for `typeId` |

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
  "metadata": {},
  "type": {
    "id": "scout-x1",
    "name": "Scout-X1",
    "type": "vehicule",
    "size": "small",
    "mobility": true,
    "environments": ["desert", "forest", "mountain", "ice", "volcanic", "plain"],
    "rules": [{ "range": "hexagon", "value": 1 }],
    "capabilities": {
      "extraction": { "speed": 5, "types": ["*"] },
      "cargo": { "size": 1000 }
    },
    "description": "Can explore, extract, and build small structures.",
    "speed": 1,
    "metadata": {}
  }
}
```

Hex coordinates exist only in `location` for v1. Clients filter by hex in application code if needed.

---

## PostgreSQL storage

**Store:** PostgreSQL (TypeORM). **Not** MongoDB. **Redis** not required for v1.

### Table: `unit_instances`

| Column | Type | Description |
| ------ | ---- | ----------- |
| `id` | UUID | Primary key |
| `type_id` | string | FK → `unit_type.id` (catalog string id) |
| `owner_id` | UUID | FK → `players.id` |
| `location` | JSONB | Canonical `Location` |
| `place_level` | enum | `cube` \| `starSystem` \| `planet` |
| `cube_id` | UUID | Always set |
| `star_system_id` | UUID | Set only when `place_level = starSystem` |
| `planet_id` | UUID | Set only when `place_level = planet` |
| `status` | enum | Stored; not used in list filters for v1 |
| `metadata` | JSONB | Optional |
| `created_at` | timestamptz | |
| `updated_at` | timestamptz | Updated on create, move, deploy, destroy, and any instance mutation |

Denormalized columns are **server-only**. They are **not** exposed in the API; responses use `location` plus embedded `type`.

### Denormalized columns by `place_level`

| `place_level` | `cube_id` | `star_system_id` | `planet_id` |
| ------------- | --------- | ---------------- | ----------- |
| `cube` | yes | — | — |
| `starSystem` | yes | yes | — |
| `planet` | yes | — | yes |

**Rules:**

- `location` JSONB is **canonical**. Denormalized fields are recomputed in the **same transaction** on every create or location update.
- On planet depth, `location` **must** include `hex_coords` and `planet.position` ([unit profile](../location.md)). **`hex_q` / `hex_r` are not denormalized.**
- When `place_level = planet`, **`star_system_id` is omitted** from denormalized columns (parent system remains in `location.starSystem.id`).
- A unit exists at **one** depth only — never both `starSystem` and `planet` denormalized shape at once.
- **`place_level`** is derived from `location` using the same depth rules as [location.md](../location.md) (equivalent to `getLocationDepth` in code) — not from `environments`.

### Indexes (v1)

| Index | Purpose |
| ----- | ------- |
| `(owner_id)` | List units by player |
| `(planet_id)` | List units on a planet — query with `place_level = 'planet'` |
| `(star_system_id)` | List units in a system — query with `place_level = 'starSystem'` |
| `(cube_id)` | List units in a cube — query with `place_level = 'cube'` |

Optional partial indexes (e.g. `WHERE place_level = 'planet'`) may be added at implementation time.

**Not used for v1:** `(owner_id, status)`, `(planet_id, hex_q, hex_r)`.

---

## Query semantics

Always filter by **`place_level`** when listing by place. Parent ids do **not** imply children.

| Access pattern | SQL filter |
| -------------- | ---------- |
| By player | `owner_id = ?` |
| By planet | `place_level = 'planet' AND planet_id = ?` |
| By stellar system | `place_level = 'starSystem' AND star_system_id = ?` — **excludes** units on planets in that system |
| By cube | `place_level = 'cube' AND cube_id = ?` — **excludes** units in systems or on planets inside that cube |
| By hex | **Not exposed as REST for v1.** Hex is read from `location.planet.hex_coords` after a planet list, or on a single instance |

**Status:** not used in list filters for v1. All instances match regardless of `status`. Clients may hide `destroyed` units locally until server-side status filtering is added in a later version.

### Planned REST endpoints (v1 — read only)

System **data** remains under `/infinity/galaxy/systems/{systemId}`; unit lists use a dedicated sub-resource under `/infinity/systems/.../units`.

| Method | Path | Auth | Returns |
| ------ | ---- | ---- | ------- |
| `GET` | `/infinity/players/me/units` | JWT | `UnitInstanceWithType[]` — caller’s owned instances |
| `GET` | `/infinity/units?ownerId={uuid}` | JWT + admin | `UnitInstanceWithType[]` — any player (admin tooling) |
| `GET` | `/infinity/planets/{planetId}/units` | public | `UnitInstanceWithType[]` — same visibility as planet surface |
| `GET` | `/infinity/systems/{starSystemId}/units` | JWT | `UnitInstanceWithType[]` |

**Deferred beyond v1:** `GET /infinity/units/{instanceId}` (single-instance detail).

Cube-scoped listing **to be defined** when Galaxy View needs it.

### Pagination (v1)

List endpoints return **unbounded arrays** (no `page` / `count`). Add pagination when planet or owner lists grow large enough to require it.

### Write operations (deferred)

Create, move, and destroy are **not** exposed as REST or Socket.IO routes in v1. Lifecycle rules below apply when a dedicated instance service is implemented (manual seeding or a later API version).

---

## Create validation

On deploy / create:

1. **`typeId`** must exist in `unit_type` (catalog).
2. **`location`** must pass the **`unit`** validation profile.
3. **`environments`** on the type must allow the placement (see mapping below).
4. **Planet placement:** server resolves the hex **biome** from the planet surface (planets / resources services; see [resources.md](../../contracts/resources.md)) and checks it against the type’s biome list.
5. **Biome-only types** (e.g. [Scout-X1](catalog/scout-x1.md)): creation and presence at **`cube`** or **`starSystem`** depth are **not allowed**.

After validation, persist `location` and recompute denormalized columns.

### `environments` ↔ `place_level`

Catalog `environments` tokens use snake_case place names; storage and queries use camelCase `place_level`.

| `environments` token | Allowed `place_level` |
| -------------------- | --------------------- |
| `cube` | `cube` |
| `stellar_system` | `starSystem` |
| biome names (e.g. `forest`, `desert`) | `planet` — hex required; hex biome must be in the type list |

**Mixed lists** (e.g. `["forest", "stellar_system"]`): placement is allowed at **any** listed depth. At deploy time, the instance’s `place_level` (from `location`) must match **one** applicable token — biome rules apply only when `place_level = planet`; place tokens apply only at their depth.

**Empty or omitted `environments`:** reject create — every catalog type must declare at least one environment token. Aligns with [units.md](units.md) (no silent “any depth” default).

---

## Lifecycle

| Event | `status` | `location` | Denormalized columns | `updated_at` |
| ----- | -------- | ---------- | -------------------- | ------------ |
| Create / deploy | `inactive` (default) | Set | Recomputed | Set |
| Move (same planet, new hex) | unchanged | Update hex + in-hex position | Unchanged (`planet_id`, `place_level` same) | Set |
| Move (change place level) | unchanged | Full transition | Recomputed | Set |
| Destroy | → `destroyed` | Unchanged (remains at last location) | Unchanged | Set |

**Ownership transfer:** not supported — `owner_id` is fixed at creation.

**Player presence (planned):** owning a unit at **any** `place_level` grants presence at that place. See [game-rules.md](../../contracts/game-rules.md).

---

## Sync rule

Only a dedicated unit location / instance service updates instances — not controllers directly.

```text
write(location) → validate(unit profile) → validate(environments) → compute(place_level, cube_id, …) → UPDATE row
```

Reject the transaction if `location` is invalid; never write partial denormalized state.

---

## Related documents

- [units.md](units.md) — `UnitType`, `UnitInstance`, capabilities, rules
- [catalog.md](catalog.md) — unit type catalog
- [location.md](../location.md) — `Location` shape and **`unit`** profile
- [game-rules.md](../../contracts/game-rules.md) — presence and movement
- [resources.md](../../contracts/resources.md) — biome / resource context for validation

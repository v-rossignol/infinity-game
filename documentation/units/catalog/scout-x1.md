# Scout-X1

```yaml
date: 2026-06-21
author: Roro LeSage
model: Composer
sources:
  - documentation/units/units.md
  - contracts/game-rules.md
  - contracts/resources.md
modified:
  - date: 2026-06-21
    author: Roro LeSage
    note: Set environments to all planet biomes except ocean
  - date: 2026-06-21
    author: Roro LeSage
    note: Set speed to 1
  - date: 2026-06-21
    author: Roro LeSage
    note: Add extraction capability (speed 5, all resources)
  - date: 2026-06-21
    author: Roro LeSage
    note: Add cargo capability (size 1000)
  - date: 2026-06-21
    author: Roro LeSage
    note: Add UnitRule (range hexagon, value 1)
```

## Overview

**Scout-X1** is the first **vehicule** entry in the [unit catalog](../catalog.md). It can explore, extract resources, and build small structures.

This entry defines a **`UnitType`** only. Runtime **`UnitInstance`** data is created when a player deploys a Scout-X1. See [units.md](../units.md).

**Status:** draft — values marked **to be defined** are open for design.

---

## Identity

| Field | Value |
| ----- | ----- |
| `id` | `scout-x1` |
| `name` | `Scout-X1` |
| `type` | `vehicule` |

---

## Unit type

| Field | Value | Notes |
| ----- | ----- | ----- |
| `id` | `scout-x1` | Stable catalog identifier. |
| `name` | `Scout-X1` | Display name. |
| `type` | `vehicule` | |
| `size` | `small` | |
| `mobility` | `true` | |
| `environments` | `desert`, `forest`, `mountain`, `ice`, `volcanic`, `plain` | Planet biomes only; **`ocean` excluded**. See [game-rules.md](../../../contracts/game-rules.md). |
| `rules` | `[{ "range": "hexagon", "value": 1 }]` | See [units.md](../units.md) → `UnitRule`. |
| `capabilities` | `{ "extraction": { "speed": 5, "types": ["*"] }, "cargo": { "size": 1000 } }` | All catalog resources; cargo capacity 1000. |
| `description` | Can explore, extract, and build small structures. | |
| `speed` | `1` | Movement speed within the current place. |
| `metadata` | `{}` | |

### `UnitType` payload

```json
{
  "id": "scout-x1",
  "name": "Scout-X1",
  "type": "vehicule",
  "size": "small",
  "mobility": true,
  "environments": ["desert", "forest", "mountain", "ice", "volcanic", "plain"],
  "rules": [
    {
      "range": "hexagon",
      "value": 1
    }
  ],
  "capabilities": {
    "extraction": {
      "speed": 5,
      "types": ["*"]
    },
    "cargo": {
      "size": 1000
    }
  },
  "description": "Can explore, extract, and build small structures.",
  "speed": 1,
  "metadata": {}
}
```

---

## Role

Early-game vehicule for planet-surface exploration, resource extraction, and deployment of small structures.

---

## Related documents

- [catalog.md](../catalog.md) — unit type catalog index
- [units.md](../units.md) — `UnitType` and `UnitInstance` specification
- [location.md](../../location.md) — placement and validation profiles
- [game-rules.md](../../../contracts/game-rules.md) — vehicule movement rules
- [resources.md](../../../contracts/resources.md) — resource catalog for `extraction.types`

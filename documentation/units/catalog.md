# Unit catalog

```yaml
date: 2026-06-21
author: Roro LeSage
model: Composer
sources:
  - documentation/units/units.md
```

## Overview

Catalog of **`UnitType`** definitions for Infinity. Each entry is an immutable unit template; players receive **`UnitInstance`** objects built from these types at runtime.

Catalog entries may define **`capabilities`** (cargo, extraction, build, storage, etc.) — see [units.md](units.md) → `capabilities`.

**Status:** draft — not enforced by Infinity Server today.

Field definitions: [units.md](units.md).

---

## Vehicules

| `id` | Name | Size | Document |
| ---- | ---- | ---- | -------- |
| `scout-x1` | Scout-X1 | `small` | [scout-x1.md](catalog/scout-x1.md) |

---

## Buildings

| `id` | Name | Size | Document |
| ---- | ---- | ---- | -------- |
| — | — | — | — |

---

## Related documents

- [units.md](units.md) — unit model (`UnitType`, `UnitInstance`)
- [game-rules.md](../../contracts/game-rules.md) — gameplay rules

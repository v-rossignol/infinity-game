---
title: Sawmill
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - buildings
  - sawmill
sources:
  - ../../../contracts/units.md
---

# `sawmill` — Sawmill

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `forest` |
| Rules | — |
| Recipe | `wood` × 25, `stone` × 100; work `100` |

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `1`, types `["wood"]` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |

Description: *small forest structure that extracts wood.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions

---
title: Quarry
author: Composer
created: 2026-07-08
updated: 2026-07-08
status: draft
tags:
  - units
  - buildings
  - quarry
sources:
  - ../../../contracts/units.md
---

# `quarry` — Quarry

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `mountain` |
| Rules | — |
| Recipe | `wood` × 100, `stone` × 50; work `150` |

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `1`, types `["stone"]` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |

Description: *small mountain structure that extracts stone.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions

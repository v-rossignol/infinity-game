---
title: Mine
author: Composer
created: 2026-07-14
updated: 2026-07-20
status: draft
tags:
  - units
  - buildings
  - mine
  - extraction
sources:
  - ../../../contracts/units.md
  - ../../../infinity/src/modules/units/constants/unit-catalog.ts
---

# `mine` — Mine

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `mountain`, `volcanic` |
| Rules | — |
| Recipe | `wood` × 200, `stone` × 300; work `250` |

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1500` |
| `extraction` | speed `1`, types `["iron-ore", "copper-ore", "silver-ore", "gold-ore", "coal", "industrial-diamonds"]` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |

Description: *Structure that extracts raw ores from the environment. Requires mountainous or volcanic terrain to operate efficiently.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../../resources/permanent-resources.md](../../resources/permanent-resources.md) — permanent ore ids (`iron-ore`, `copper-ore`, `coal`)
- [../../resources/occasional-resources.md](../../resources/occasional-resources.md) — occasional ore ids (`gold-ore`, `silver-ore`, `industrial-diamonds`)

---
title: Forge
author: Composer
created: 2026-07-11
updated: 2026-07-11
status: draft
tags:
  - units
  - buildings
  - forge
  - transformation
sources:
  - ../../../contracts/units.md
  - ../../../infinity/src/modules/units/constants/unit-catalog.ts
---

# `forge` — Forge

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `plain`, `forest`, `mountain`, `desert`, `ice`, `volcanic` |
| Rules | — |
| Recipe | `wood` × 150, `stone` × 250; work `200` |

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `garage` | `small` × 1, `medium` × 0, `large` × 0 |
| `transformation` | speed `1`, types `["iron-ingot", "copper-ingot"]` |

Description: *Small structure that transforms ore into iron and copper ingots.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../../resources/transformed-resources.md](../../resources/transformed-resources.md) — transformed product ids (`iron-ingot`, `copper-ingot`)

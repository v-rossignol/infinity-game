---
title: Dock
author: Composer
created: 2026-07-14
updated: 2026-07-14
status: draft
tags:
  - units
  - buildings
  - dock
  - compound-environment
sources:
  - ../../../contracts/units.md
  - ../../../infinity/src/modules/units/constants/unit-catalog.ts
  - ../../places/planets/hexagons/building-zones.md
---

# `dock` — Dock

**Implemented** · `building` · `small`

| Field | Value |
| ----- | ----- |
| Mobility | `false` |
| Speed | `null` |
| Environments | `ocean+plain`, `ocean+desert`, `ocean+forest`, `ocean+ice` |
| Rules | — |
| Recipe | `wood` × 200, `stone` × 200; work `250` |

## Placement

Each environment entry uses **compound** syntax (`biomeA+biomeB`): the dock may only be built on a **side building zone** where one adjacent hex matches the first biome and the neighbor hex across that zone matches the second (order-independent). See [building-zones.md](../../places/planets/hexagons/building-zones.md#side-zones).

| Environment | Meaning |
| ----------- | ------- |
| `ocean+plain` | Side zone between ocean and plain |
| `ocean+desert` | Side zone between ocean and desert |
| `ocean+forest` | Side zone between ocean and forest |
| `ocean+ice` | Side zone between ocean and ice |

Central zones and side zones whose biomes do not match one of these pairs are rejected by the server build API.

## Capabilities

| Capability | Value |
| ---------- | ----- |
| `cargo` | size `1000` |
| `extraction` | speed `1`, types `["food", "salt-water"]` |
| `garage` | `small` × 2, `medium` × 0, `large` × 0 |

Description: *Coastal dock built near the ocean.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../../places/planets/hexagons/building-zones.md](../../places/planets/hexagons/building-zones.md) — side zones and neighbor pairing

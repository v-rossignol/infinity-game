---
title: Mine
author: Composer
created: 2026-07-14
updated: 2026-07-14
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

**Planned** · `building` · `small`

   Field          | Value                          |
 | -------------- | ------------------------------ |
 | Mobility       | `false`                        |
 | Speed          | `null`                         |
 | Environments   | `mountain`, `volcanic`         |
 | Rules          | —                              |
 | Recipe         | `wood` × 200, `stone` × 300; work `250` |

## Capabilities

 | Capability         | Value                          |
 | ------------------ | ------------------------------ |
 | `cargo`            | size `1500`                    |
 | `garage`           | `small` × 1, `medium` × 0, `large` × 0 |
 | `extraction`       | speed `1`, types `[ "iron-ore", "copper-ore", "silver-ore", "gold-ore", "coal", "industrial-diamonds" ]` |

Description: *Structure that extracts raw ores from the environment. Requires mountainous or volcanic terrain to operate efficiently.*

## Related

- [index.md](index.md) — buildings catalog
- [../capabilities.md](../capabilities.md) — capability key definitions
- [../../resources/raw-resources.md](../../resources/raw-resources.md) — raw ore product ids (`iron-ore`, `copper-ore`, `gold-ore`)
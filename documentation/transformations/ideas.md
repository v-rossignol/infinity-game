# Transformations in Infinity

```yaml
date: 2026-07-05
author: Roro LeSage
model: Vibe
sources:
  - contracts/resources.md
  - contracts/units.md
```

## Overview

Draft ideas for **resource transformations** on planetary surfaces — converting terrain resources (and derived goods) into other products. Inputs use resource ids from [resources.md](../../contracts/resources.md) where they exist today; outputs marked *new* are not yet in the resource catalog.

**Status:** draft — not implemented by Infinity Server.

Transformation types:

| Type | Description |
| ---- | ----------- |
| Simple transformation | One primary input → one output |
| Combination | Multiple inputs → one output |
| Advanced refining | Higher-tier processing, often energy- or fuel-intensive |
| Synthesis | Assembly into usable objects or advanced materials |

---

## Metals

| Type | Transformation | Inputs | Output | Suggested quantity | Notes |
| ---- | -------------- | ------ | ------ | ------------------ | ----- |
| Simple transformation | Copper ingot | `copper-ore` (10) + `coal` (3) | Copper ingot | 7 | Intermediate step for alloys. New. |
| Simple transformation | Iron ingot | `iron-ore` (10) + `coal` (5) | Iron ingot | 8 | Intermediate step for alloys. New. |
| Simple transformation | Molten iron | `iron-ore` (10) + `coal` (5) | Molten iron | 8 | Alternative to iron ingot. New. |
| Combination | Bronze | Copper ingot (7) + `coal` (2) | Bronze ingot | 5 | Copper alloy. New. |
| Combination | Steel | Iron ingot (8) + `coal` (8) | Steel ingot | 6 | Existing (to implement). |
| Advanced refining | Stainless steel | Steel ingot (6) + Chromium* | Stainless steel | 4 | Add chromium as an occasional resource. New. |
| Advanced refining | Heat-resistant steel | Iron ingot (6) + `basalt` (5) | Heat-resistant steel | 4 | New. |
| Advanced refining | Copper-gold alloy | Copper ingot (7) + `gold`* | Electrum | 6 | `gold` as an occasional resource. New. |
| Synthesis | Iron parts | Iron ingot (4) + `wood` (2) | Iron tools | 1 | Usable object. New. |
| Synthesis | Steel parts | Steel ingot (3) + `wood` (1) | Steel tools | 1 | Usable object. New. |
| Synthesis | Gunpowder | `coal` (5) + `sulfur` (5) + `nitre`* | Gunpowder | 3 | `nitre` available on desert terrain. New. |
| Synthesis | Salt battery | `alkaline-minerals` (10) + Copper ingot (3) | Salt battery | 2 | New (energy storage). |

---

## Transformation ideas

| Terrain | Type | Transformation | Inputs | Output | Suggested quantity | Terrains involved | Notes |
| ------- | ---- | -------------- | ------ | ------ | ------------------ | ----------------- | ----- |
| `plain` | Simple transformation | Food dehydration | `food` (5) | Dried food | 3 | `plain` | Long-term preservation. New resource. |
| `plain` | Simple transformation | Water purification | `fresh-water` (10) | Purified water | 8 | `plain` | For consumption or medical use. New. |
| `plain` | Combination | Flour | `food` (10) + `stone` (2) | Flour | 6 | `plain`, `mountain` | Uses stone millstones. New. |
| `plain` | Advanced refining | Bioethanol | `food` (20) + `fresh-water` (5) | Bioethanol | 10 | `plain` | Fuel or disinfectant. New. |
| `plain` | Synthesis | Organic fertilizer | `food` (5) + `fresh-water` (5) | Organic fertilizer | 10 | `plain` | Improves crop yields. New. |
| `forest` | Simple transformation | Charcoal | `wood` (20) | Charcoal | 15 | `forest` | Fuel or metallurgy reducing agent. New. |
| `forest` | Simple transformation | Wooden planks | `wood` (10) | Wooden planks | 8 | `forest` | For construction. Existing (to confirm). |
| `forest` | Combination | Paper | `wood` (5) + `fresh-water` (2) | Paper | 4 | `forest`, `plain` | New. |
| `forest` | Combination | Plant glue | `wood` (5) + `fresh-water` (1) | Plant glue | 3 | `forest`, `plain` | For crafting or construction. New. |
| `forest` | Advanced refining | Wood tar | `wood` (30) | Wood tar | 10 | `forest` | Waterproofing or fuel. New. |
| `forest` | Synthesis | Basic furniture | `wood` (50) + `stone` (10) | Basic furniture | 1 | `forest`, `mountain` | Usable object. New mechanic. |
| `forest` | Synthesis | Paper pulp | `wood` (10) + `alkaline-minerals` (2) | Pulp | 8 | `forest`, `desert` | Uses natural alkali. New. |
| `mountain` | Simple transformation | Cut stone | `stone` (20) | Cut stone | 15 | `mountain` | For construction. New. |
| `mountain` | Simple transformation | Copper ingot | `copper-ore` (10) + `coal` (3) | Copper ingot | 7 | `mountain` | Intermediate step for alloys. New. |
| `mountain` | Simple transformation | Iron ingot | `iron-ore` (10) + `coal` (5) | Iron ingot | 8 | `mountain` | Intermediate step for alloys. New. |
| `mountain` | Simple transformation | Molten iron | `iron-ore` (10) + `coal` (5) | Molten iron | 8 | `mountain` | Alternative to iron ingot. New. |
| `mountain` | Combination | Bronze | Copper ingot (7) + `coal` (2) | Bronze ingot | 5 | `mountain` | Copper alloy. New. |
| `mountain` | Combination | Steel | Iron ingot (8) + `coal` (8) | Steel ingot | 6 | `mountain` | Existing (to implement). |
| `mountain` | Combination | Brick | `stone` (10) + `coal` (2) | Brick | 8 | `mountain` | For construction. New. |
| `mountain` | Advanced refining | Cement | `stone` (30) + `coal` (10) | Cement | 20 | `mountain` | New. |
| `mountain` | Advanced refining | Stainless steel | Steel ingot (6) + Chromium* | Stainless steel | 4 | `mountain`, `plain`* | Add chromium as an occasional resource. New. |
| `mountain` | Synthesis | Stone tools | `stone` (15) + `wood` (5) | Stone tools | 1 | `mountain`, `forest` | Usable object. New mechanic. |
| `mountain` | Synthesis | Gunpowder | `coal` (5) + `sulfur` (5) + `nitre`* | Gunpowder | 3 | `mountain`, `desert` | `nitre` available on desert terrain. New. |
| `desert` | Simple transformation | Glass | `silica` (50) + `coal` (10) | Glass | 30 | `desert` | New. |
| `desert` | Simple transformation | Sodium hydroxide (NaOH) | `alkaline-minerals` (10) | Sodium hydroxide | 5 | `desert` | For soap-making or chemistry. New. |
| `desert` | Combination | Silica cement | `silica` (20) + `stone` (10) | Silica cement | 15 | `desert`, `mountain` | Heat-resistant. New. |
| `desert` | Advanced refining | Fiberglass | `silica` (100) + `coal` (20) | Fiberglass | 50 | `desert` | For composites. New. |
| `desert` | Synthesis | Soap | `alkaline-minerals` (5) + `food` (5) | Soap | 3 | `desert`, `plain` / `ocean` | New. |
| `desert` | Synthesis | Salt battery | `alkaline-minerals` (10) + Copper ingot (3) | Salt battery | 2 | `desert`, `mountain` | New (energy storage). |
| `ocean` | Simple transformation | Desalination | `salt-water` (50) + `coal` (10) | `fresh-water` | 30 | `ocean` | New. |
| `ocean` | Simple transformation | Salt | `salt-water` (20) | Salt | 10 | `ocean` | New. |
| `ocean` | Combination | Dried algae | `food` (10, ocean) | Dried algae | 5 | `ocean` | For food or fertilizer. New. |
| `ocean` | Advanced refining | Algae biofuel | `food` (30, ocean) + `coal` (5) | Algae biofuel | 15 | `ocean` | New. |
| `ocean` | Synthesis | Beads or jewelry | `salt-water` (10) + `obsidian`* | Glass beads | 5 | `ocean`, `volcanic` | `obsidian` from volcanic terrain. New. |
| `ice` | Simple transformation | Fresh water (melt) | `ice` (100) | `fresh-water` | 90 | `ice` | Existing (equivalent to `fresh-water`). |
| `ice` | Simple transformation | Liquid nitrogen | `cryogenic-materials` (10) | Liquid nitrogen | 5 | `ice` | Tie to cryogenic gases. New. |
| `ice` | Combination | Dry ice (solid CO₂) | `ice` (50) + `coal` (10) | Dry ice | 20 | `ice`, `mountain` | For refrigeration. New. |
| `ice` | Advanced refining | Hydrogen | `ice` (100) + Energy* | Hydrogen | 30 | `ice` | Energy source TBD (e.g. `oil` or charcoal). New. |
| `ice` | Synthesis | Synthetic methane | `cryogenic-materials` (5) + `coal` (10) | Synthetic methane | 8 | `ice`, `mountain` | New. |
| `volcanic` | Simple transformation | Rock wool | `basalt` (20) | Rock wool | 15 | `volcanic` | Thermal insulation. New. |
| `volcanic` | Simple transformation | Purified sulfur | `sulfur` (10) | Purified sulfur | 8 | `volcanic` | For chemistry or explosives. New. |
| `volcanic` | Combination | Heat-resistant steel | Iron ingot (6) + `basalt` (5) | Heat-resistant steel | 4 | `volcanic`, `mountain` | New. |
| `volcanic` | Combination | Volcanic glass | `obsidian` (10) + `silica` (5) | Volcanic glass | 8 | `volcanic`, `desert` | New. |
| `volcanic` | Advanced refining | Sulfuric acid | `sulfur` (10) + `fresh-water` (5) | Sulfuric acid | 5 | `volcanic`, `plain` / `ocean` | For chemical industry. New. |
| `volcanic` | Synthesis | Explosives (black powder) | `sulfur` (5) + `coal` (5) + `nitre`* | Black powder | 3 | `volcanic`, `desert` | `nitre` from desert terrain. New. |
| Multi-terrain | Combination | Reinforced concrete | `stone` (30) + Iron ingot (6) + `coal` (5) | Reinforced concrete | 20 | `mountain`, `plain` / `desert` | New. |
| Multi-terrain | Combination | Ceramics | `silica` (20) + `alkaline-minerals` (5) | Ceramics | 15 | `desert`, `mountain` | New. |
| Multi-terrain | Advanced refining | Copper-gold alloy | Copper ingot (7) + `gold`* | Electrum | 6 | `mountain`, `plain`* | `gold` as an occasional resource. New. |
| Multi-terrain | Synthesis | Solar panels | `silica` (50) + Copper ingot (3) | Solar panels | 5 | `desert`, `mountain` | New (energy generation). |
| Multi-terrain | Synthesis | Lithium batteries | `lithium`* + Copper ingot (3) | Lithium battery | 2 | `desert` | `lithium` as an occasional resource. New. |

\* Resource or derivative not yet in the terrain catalog — see [resources.md](../../contracts/resources.md) (occasional resources planned).

---

## Related documents

- [resources.md](../../contracts/resources.md) — terrain resource ids and biomes
- [units.md](../../contracts/units.md) — unit capabilities (extraction, build, cargo)
- [game-rules.md](../../contracts/game-rules.md) — gameplay rules

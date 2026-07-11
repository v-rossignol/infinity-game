---
title: Spawn
author: Composer
created: 2026-07-09
updated: 2026-07-09
status: draft
tags:
  - spawn
  - players
  - first-entry
  - home-planet
sources:
  - ../../contracts/game-rules.md
  - ../../contracts/game-api.yaml
---

# Spawn

Knowledge domain for **player spawn** — special features and rules tied to first entry, home planet assignment, and starter grants.

## Purpose

Organize spawn-related gameplay and implementation notes that go beyond a single contract section: how a new player enters the galaxy, where they land, and what they receive at spawn time.

## Documents

| Document | Description |
| -------- | ----------- |
| [first-entry.md](first-entry.md) | First spawn flow, `enter-game` API, and hex assignment |
| [home-planet.md](home-planet.md) | Home planet identity, spawn surface adjustments, vs current presence |
| [starter-grants.md](starter-grants.md) | Units and assets granted on first spawn |

## Authoritative sources

| Document | Role |
| -------- | ---- |
| [contracts/game-rules.md](../../contracts/game-rules.md) | First entry, home planet, spawn selection, starter vehicule |
| [contracts/game-api.yaml](../../contracts/game-api.yaml) | `POST /players/me/enter-game` — idempotent spawn or resume |
| [contracts/units.md](../../contracts/units.md) | Units granted on first spawn (e.g. Scout-X1) |

## Related domains

- [units/](../units/) — unit catalog and capabilities (starter vehicule)
- [resources/](../resources/) — terrain resources on the spawn hex
- [OKF root](../index.md)

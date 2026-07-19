# Log — resources

## 2026-07-14

- Renamed occasional resource ids `gold` → `gold-ore` and `silver` → `silver-ore` in `occasional-resources.md` (display names: Gold ore, Silver ore).

## 2026-07-11

- Aligned occasional resource quantities in `packages/shared-config/src/terrain-resources.ts` and `contracts/resources.md` with `occasional-resources.md`.
- Aligned `occasional-resources.md` terrain summary: forest occasional **Bauxite** (was Bauxite deposits; matches catalog).

## 2026-07-09

- Renamed occasional resource `brine-lithium` / Brine lithium → `lithium` / Lithium across OKF, shared-config, contracts, and documentation.
- Removed `contracts/resources.md` from `index.md` sources and authoritative catalogs; this domain is now the resource entry point.
- Aligned `oil` occasional resource: id `oil`, quantity 10 (was `crude-oil` / 5–10 range in OKF; matches `packages/shared-config/src/terrain-resources.ts`).

## 2026-07-08

- Added `permanent-resources.md`, `occasional-resources.md`, and `transformed-resources.md` (from [contracts/resources.md](../../contracts/resources.md)).
- Updated `index.md` and `terrain-resources.md` with category links.
- Fixed `index.md` metadata: YAML front matter instead of fenced code block.

## 2026-07-06

- Created domain: `index.md`, `log.md`.
- Added `terrain-resources.md` (from [contracts/game-rules.md](../../contracts/game-rules.md)).
- Added `units-and-resources.md` (from [contracts/units.md](../../contracts/units.md)).

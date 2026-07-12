# Hex subdivision proposal

```yaml
title: Hex subdivision proposal
author: Roro LeSage
model:
  - name: OpenAI ChatGPT
  - version: GPT-5.5
created: 2026-07-11
updated: 2026-07-11
status: proposal
tags:
  - places
  - planets
  - hexagons
  - geometry
  - subdivision
```

## Goal

Replace the current **6 × 6** build grid with a simpler subdivision based on **equal-sized squares**.

Objectives:

- provide a central **3 × 3** square grid for most gameplay;
- provide one square centered on each side of the hexagon to represent the six neighboring directions;
- keep every square the same size.

This subdivision is intended as a logical representation rather than a space-filling tiling.

---



## Hexagon

The hexagon is the existing normalized local polygon:


| Vertex      | Position    |
| ----------- | ----------- |
| Top         | `(0.5, 0)`  |
| Upper right | `(1, 0.25)` |
| Lower right | `(1, 0.75)` |
| Bottom      | `(0.5, 1)`  |
| Lower left  | `(0, 0.75)` |
| Upper left  | `(0, 0.25)` |


The center is `(0.5, 0.5)`.

---



## Square size

The side length of every square is:

```text
squareSide = hexSide / 3
```

where

```text
hexSide = √5 / 4 ≈ 0.559016994
```

Therefore

```text
squareSide ≈ 0.186338998
```

---



## Central 3 × 3 grid

The central grid is centered on `(0.5, 0.5)`.

The square centers are:


| x        | y        | rotation |
| -------- | -------- | -------- |
| 0.313661 | 0.313661 | 0°       |
| 0.500000 | 0.313661 | 0°       |
| 0.686339 | 0.313661 | 0°       |
| 0.313661 | 0.500000 | 0°       |
| 0.500000 | 0.500000 | 0°       |
| 0.686339 | 0.500000 | 0°       |
| 0.313661 | 0.686339 | 0°       |
| 0.500000 | 0.686339 | 0°       |
| 0.686339 | 0.686339 | 0°       |


---



## Side squares

Each side receives one square.

The center of the square is placed exactly at the midpoint of the side.

The side itself passes through the center of the square, so half of the square lies inside the hexagon and half outside.


| Side        | Center          | Rotation   |
| ----------- | --------------- | ---------- |
| Upper right | `(0.75, 0.125)` | `26.565°`  |
| Right       | `(1.0, 0.5)`    | `90°`      |
| Lower right | `(0.75, 0.875)` | `-26.565°` |
| Lower left  | `(0.25, 0.875)` | `26.565°`  |
| Left        | `(0.0, 0.5)`    | `90°`      |
| Upper left  | `(0.25, 0.125)` | `-26.565°` |


---



## Layout

```text
             ▲

          ◇

      □ □ □

◇     □ □ □     ◇

      □ □ □

          ◇

             ▼
```

- `□` = central square
- `◇` = side square

---



## Gameplay rationale

The subdivision naturally separates two concepts:

- the **3 × 3** central grid represents the usable area inside the hex;
- the **6 side squares** represent the six connections with neighboring hexes.

The side squares provide explicit entry/exit points and directional interaction without requiring additional indexing.

---



## Notes

This subdivision is not intended to tile the hexagon perfectly.

It is a gameplay-oriented logical decomposition with:

- 9 central cells;
- 6 directional interface cells;
- identical square size everywhere.


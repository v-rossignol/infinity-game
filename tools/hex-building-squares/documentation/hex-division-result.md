---
title: Hex subdivision proposal
author: OpenAI ChatGPT
model: GPT-5.5
generated: 2026-07-11
status: proposal
tags:
  - places
  - planets
  - hexagons
  - geometry
  - subdivision
---

# Hex Subdivision Proposal

## Goal

Replace the current **6 × 6** build grid with a simpler subdivision based on **equal-sized squares**.

Objectives:

- provide a central **3 × 3** square grid for most gameplay;
- provide one square centered on each side of the hexagon to represent the six neighboring directions;
- keep every square the same size.

This subdivision is intended as a logical representation rather than a space-filling tiling.

---

# Hexagon

The subdivision uses the existing normalized hexagon.

| Vertex | Position |
| ------- | -------- |
| Top | `(0.5, 0.0)` |
| Upper Right | `(1.0, 0.25)` |
| Lower Right | `(1.0, 0.75)` |
| Bottom | `(0.5, 1.0)` |
| Lower Left | `(0.0, 0.75)` |
| Upper Left | `(0.0, 0.25)` |

The hexagon center is:

```text
(0.5, 0.5)
```

---

# Square Size

A fixed square size is used throughout the subdivision.

```ts
export const HEX_SQUARE_SIZE = 0.175;
```

All fifteen squares have this same side length.

---

# Central Grid

The central grid is a regular **3 × 3** arrangement centered on the hexagon.

The square centers are:

| x | y | Rotation |
|---:|---:|---:|
| 0.325 | 0.325 | 0° |
| 0.500 | 0.325 | 0° |
| 0.675 | 0.325 | 0° |
| 0.325 | 0.500 | 0° |
| 0.500 | 0.500 | 0° |
| 0.675 | 0.500 | 0° |
| 0.325 | 0.675 | 0° |
| 0.500 | 0.675 | 0° |
| 0.675 | 0.675 | 0° |

The grid occupies a square of size:

```text
0.525 × 0.525
```

leaving comfortable margins around it.

---

# Side Squares

Each side of the hexagon contains one additional square.

The center of each square is located at the midpoint of its corresponding side.

The side itself passes through the center of the square, so approximately half of the square lies inside the hexagon and half outside.

| Side | Center | Rotation |
|------|--------|---------:|
| Upper Right | `(0.75, 0.125)` | `26.565°` |
| Right | `(1.00, 0.50)` | `90°` |
| Lower Right | `(0.75, 0.875)` | `-26.565°` |
| Lower Left | `(0.25, 0.875)` | `26.565°` |
| Left | `(0.00, 0.50)` | `90°` |
| Upper Left | `(0.25, 0.125)` | `-26.565°` |

---

# Layout

```text
               ▲

            ◇

        □ □ □

◇       □ □ □       ◇

        □ □ □

            ◇

               ▼
```

Legend:

- `□` = central square
- `◇` = side square

---

# Cell Count

The subdivision contains:

- **9** central squares
- **6** side squares

for a total of **15 logical cells**.

---

# Gameplay Rationale

The subdivision separates two complementary concepts.

The **3 × 3** central grid is intended for gameplay elements located inside the hexagon, such as buildings, terrain features, or units.

The **6 side squares** explicitly represent the six neighboring hexagons. They naturally provide:

- movement entry/exit points;
- directional interactions;
- road or network connections;
- propagation interfaces.

This makes neighbor relationships explicit while keeping the central gameplay area regular.

---

# Notes

The chosen square size (`0.175`) is intentionally slightly smaller than the theoretical maximum.

This provides:

- visual spacing;
- tolerance against floating-point precision issues;
- room for rendering borders, highlights, and overlays;
- flexibility for future gameplay additions.

The subdivision is designed as a logical gameplay representation rather than a geometric tessellation of the hexagon.
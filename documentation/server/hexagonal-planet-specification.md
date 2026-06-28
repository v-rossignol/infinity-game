# Infinity - Hexagonal Planet Surface Specification

```yaml
date: 2026-06-08  
author: Roro LeSage  
model: Agent Infinity (Mistral Medium 3.5)  
sources:
  - input
```

---

## Overview

This document describes the **hexagonal grid-based model** for planet surfaces in the *Infinity* game. Each planet is represented as a **toroidal hexagonal grid** of odd size (e.g., 5x5, 7x7), where each hexagon contains a biome, resources, and danger levels. The grid uses **axial coordinates** (`q`, `r`) for neighbor calculations, and edges wrap around (toroidal topology).

---

## Data Model

### Hexagon Interface

Each hexagon in the grid is defined as follows:

```typescript
interface Hexagon {
  biome: 'desert' | 'forest' | 'ocean' | 'mountain' | 'ice' | 'volcanic';
  resources: Array<{
    type: string; // e.g., "iron", "gold", "natural gas"
    abundance: number; // 0-100
    rarity: 'common' | 'rare' | 'epic' | 'legendary';
  }>;
  dangerLevel: number; // 0-10
  coordinates: { q: number; r: number }; // Axial coordinates
}
```

### Planet Details Interface

A planet details is defined by the following attributes:

```typescript
interface PlanetDetails {
  _id: string; // Unique identifier (MongoDB)
  name: string; // Format: "[StarSystemName]-[PlanetNumber]"
  starSystemId: string; // Reference to the parent star system
  type: 'rocky' | 'gas' | 'ice' | 'oceanic';
  distanceToStar: number; // In Astronomical Units (AU)
  planetNumber: number; // Rounded distance to star (integer)
  hexGridSize: number; // Odd integer (e.g., 5, 7, 9)
  hexagons: Hexagon[]; // Flat list of all hexagons
  globalResources: Array<{ type: string; totalQuantity: number }>;
  position: { x: number; y: number }; // 2D coordinates in the star system
  generatedAt: Date; // Timestamp of generation
}
```

---

## Hexagonal Grid

### Grid Structure

- The grid is **square-shaped** but uses **axial coordinates** (`q`, `r`) for hexagons.
- The grid size (`hexGridSize`) is an **odd integer** (e.g., 5, 7, 9).
- The total number of hexagons is `hexGridSize * hexGridSize`.

### Toroidal Topology

- The grid edges **wrap around**:
  - The left neighbor of `(0, r)` is `(hexGridSize - 1, r)`.
  - The top neighbor of `(q, 0)` is `(q, hexGridSize - 1)`.
  - This creates a **toroidal (donut-shaped) world** where movement wraps around the edges.

---

## Neighbor Calculation

### Axial Coordinates

Each hexagon is identified by its axial coordinates `(q, r)`. The six direct neighbors of a hexagon at `(q, r)` are calculated as follows:


| Direction    | Neighbor Coordinates |
| ------------ | -------------------- |
| Top-Left     | `(q-1, r+1)`         |
| Top-Right    | `(q, r+1)`           |
| Right        | `(q+1, r)`           |
| Bottom-Right | `(q+1, r-1)`         |
| Bottom-Left  | `(q, r-1)`           |
| Left         | `(q-1, r)`           |


### Toroidal Wrapping

To handle edge wrapping, use modulo arithmetic:

```typescript
function getNeighbors(q: number, r: number, gridSize: number): { q: number; r: number }[] {
  return [
    // Top-Left
    { q: (q - 1 + gridSize) % gridSize, r: (r + 1) % gridSize },
    // Top-Right
    { q: q, r: (r + 1) % gridSize },
    // Right
    { q: (q + 1) % gridSize, r: r },
    // Bottom-Right
    { q: (q + 1) % gridSize, r: (r - 1 + gridSize) % gridSize },
    // Bottom-Left
    { q: q, r: (r - 1 + gridSize) % gridSize },
    // Left
    { q: (q - 1 + gridSize) % gridSize, r: r },
  ];
}
```

---

## Planet Generation Algorithm

### 1. Base Attributes

- **Planet Type**:
  - 60% rocky, 25% gas, 10% ice, 5% oceanic (weighted random).
- **Distance to Star**:
  - Random value between `0.5 AU` and `10 AU`.
- **Planet Number**:
  - Rounded value of `distanceToStar` (e.g., `3.2 AU` → `3`).
- **Name**:
  - Format: `[StarSystemName]-[PlanetNumber]` (e.g., `Sol-3`).

### 2. Hexagonal Grid Generation

- **Grid Size**:
  - Must be an **odd integer** (e.g., 5, 7, 9).
- **Hexagon List**:
  - A flat list of `hexGridSize * hexGridSize` hexagons.
  - Each hexagon is assigned:
    - A **biome** (based on planet type and position).
    - **Resources** (based on biome).
    - A **danger level** (random between 0 and 10).

### 3. Biome Assignment

Biomes are assigned based on:

- **Planet Type**:
  - Rocky: Mostly `forest`, `mountain`, or `desert`.
  - Ice: Mostly `ice` or `mountain`.
  - Oceanic: Mostly `ocean` or `forest`.
  - Gas: Mostly `volcanic` or `desert`.
- **Distance to Center**:
  - Hexagons closer to the center may have different biomes than those at the edges.

#### Example Biome Assignment Function

```typescript
function getBiomeForHexagon(
  planetType: 'rocky' | 'gas' | 'ice' | 'oceanic',
  distanceToStar: number,
  q: number,
  r: number,
  gridSize: number
): 'desert' | 'forest' | 'ocean' | 'mountain' | 'ice' | 'volcanic' {
  const center = (gridSize - 1) / 2;
  const distanceToCenter = Math.sqrt(
    Math.pow(q - center, 2) + Math.pow(r - center, 2)
  );
  const normalizedDistance = distanceToCenter / center; // 0 (center) to ~1.41 (corner)

  if (planetType === 'rocky') {
    if (normalizedDistance < 0.5) return weightedRandom(['forest', 'mountain'], [0.6, 0.4]);
    else if (normalizedDistance < 1.0) return weightedRandom(['desert', 'mountain'], [0.7, 0.3]);
    else return 'desert';
  } else if (planetType === 'ice') {
    if (normalizedDistance < 0.5) return weightedRandom(['ice', 'mountain'], [0.8, 0.2]);
    else return 'ice';
  } else if (planetType === 'oceanic') {
    if (normalizedDistance < 0.7) return 'ocean';
    else return weightedRandom(['forest', 'mountain'], [0.6, 0.4]);
  } else { // gas
    return weightedRandom(['volcanic', 'desert'], [0.7, 0.3]);
  }
}
```

---

## Example PlanetDetails

### Input

- **Star System Name**: `Sol`
- **Star System ID**: `sol_123`
- **Hex Grid Size**: `5`

### Output

```json
{
  "_id": "planet_456",
  "name": "Sol-3",
  "starSystemId": "sol_123",
  "type": "rocky",
  "distanceToStar": 3.2,
  "planetNumber": 3,
  "hexGridSize": 5,
  "hexagons": [
    {
      "biome": "desert",
      "resources": [
        { "type": "iron", "abundance": 80, "rarity": "common" },
        { "type": "gold", "abundance": 10, "rarity": "rare" }
      ],
      "dangerLevel": 5,
      "coordinates": { "q": 0, "r": 0 }
    },
    {
      "biome": "forest",
      "resources": [
        { "type": "wood", "abundance": 90, "rarity": "common" }
      ],
      "dangerLevel": 2,
      "coordinates": { "q": 1, "r": 0 }
    },
    // ... (23 more hexagons)
  ],
  "globalResources": [
    { "type": "iron", "totalQuantity": 5000 },
    { "type": "gold", "totalQuantity": 500 }
  ],
  "position": { "x": 3.2, "y": 0 },
  "generatedAt": "2026-06-08T00:00:00Z"
}
```

---

## Neighbor Example

For a hexagon at `(0, 0)` in a `5x5` grid, its neighbors are:


| Direction    | Coordinates |
| ------------ | ----------- |
| Top-Left     | (4, 1)      |
| Top-Right    | (0, 1)      |
| Right        | (1, 0)      |
| Bottom-Right | (1, 4)      |
| Bottom-Left  | (0, 4)      |
| Left         | (4, 0)      |


---

## Open Questions

1. **Biome Generation**: Should we use **Perlin Noise** for more natural biome distribution?
2. **Resource Distribution**: Should resources be clustered or randomly distributed?
3. **Grid Visualization**: Do you need a visual representation of the hexagonal grid for debugging?

---

## Next Steps

- Finalize biome generation logic.
- Implement the hexagonal grid rendering for the client.
- Integrate with MongoDB for persistence.
# Infinity - Planet Generation Specification

---

```yaml
date: 2026-06-08  
author: Roro LeSage  
model: Agent Infinity (Mistral Medium 3.5) 
sources: []
```

## Overview

This document describes the **data model** and **procedural generation algorithm** for planets in the *Infinity* game. Planets are dynamically generated within star systems and feature unique biomes, resources, and challenges.

---

## Data Model

### Planet Interface

A planet is defined by the following attributes:

```typescript
interface Biome {
  type: 'desert' | 'forest' | 'ocean' | 'mountain' | 'ice' | 'volcanic';
  resources: Array<{
    type: string; // e.g., "iron", "gold", "natural gas"
    abundance: number; // 0-100
    rarity: 'common' | 'rare' | 'epic' | 'legendary';
  }>;
  dangers: Array<{
    type: string; // e.g., "storm", "predators"
    intensity: number; // 1-10
  }>;
}

interface Planet {
  _id: string; // Unique identifier (MongoDB)
  name: string; // Format: "[StarSystemName]-[PlanetNumber]"
  starSystemId: string; // Reference to the parent star system
  type: 'rocky' | 'gas' | 'ice' | 'oceanic';
  distanceToStar: number; // In Astronomical Units (AU)
  planetNumber: number; // Rounded distance to star (integer)
  biomes: Biome[]; // List of biomes present on the planet
  globalResources: Array<{
    type: string;
    totalQuantity: number;
  }>;
  position: { x: number; y: number }; // 2D coordinates in the star system
  generatedAt: Date; // Timestamp of generation
}
```

---

## Procedural Generation Algorithm

### 1. Base Attributes

- **Planet Type**:
  - 60% rocky, 25% gas, 10% ice, 5% oceanic.
  - Selected using weighted randomness.
- **Distance to Star**:
  - Random value between `0.5 AU` and `10 AU`.
- **Planet Number**:
  - Rounded value of `distanceToStar` (e.g., `3.2 AU` → `3`).
- **Name**:
  - Format: `[StarSystemName]-[PlanetNumber]` (e.g., `Alpha Centauri-3`).
- **Position**:
  - `x = distanceToStar * cos(angle)`
  - `y = distanceToStar * sin(angle)`
  - `angle = random(0, 360) degrees`

---

### 2. Biome Generation

- **Number of Biomes**:
  - Random integer between 1 and 5.
- **Biome Types**:
  - Weighted random selection based on **planet type** and **distance to star**.
  - Example: A **rocky planet** at `2 AU` may have "desert" or "mountain" biomes.
  - Example: An **ice planet** at `8 AU` may have "ice" or "volcanic" biomes.
- **Resources and Dangers**:
  - Each biome has a set of possible resources and dangers, with varying abundance and intensity.

---

### 3. Global Resources

- **Legendary Resource**:
  - One legendary resource per planet (e.g., "Nyx Crystal").
- **Common Resources**:
  - Generated based on the biomes present (e.g., "sulfur" and "iron" for a volcanic biome).

---

## Pseudocode Example

```typescript
function generatePlanet(starSystemName: string, starSystemId: string): Planet {
  // Generate base attributes
  const type = weightedRandom(
    ['rocky', 'gas', 'ice', 'oceanic'],
    [0.6, 0.25, 0.1, 0.05]
  );
  const distanceToStar = random(0.5, 10);
  const planetNumber = Math.round(distanceToStar);
  const angle = random(0, 360);
  const position = {
    x: distanceToStar * Math.cos(angle),
    y: distanceToStar * Math.sin(angle),
  };

  // Generate biomes
  const biomes = [];
  const numBiomes = random(1, 5);
  for (let i = 0; i < numBiomes; i++) {
    const biomeType = weightedRandom(
      ['desert', 'forest', 'ocean', 'mountain', 'ice', 'volcanic'],
      getBiomeWeights(type, distanceToStar)
    );
    biomes.push({
      type: biomeType,
      resources: generateBiomeResources(biomeType),
      dangers: generateBiomeDangers(biomeType),
    });
  }

  // Return the planet object
  return {
    _id: generateId(),
    name: `${starSystemName}-${planetNumber}`,
    starSystemId,
    type,
    distanceToStar,
    planetNumber,
    biomes,
    globalResources: generateGlobalResources(biomes),
    position,
    generatedAt: new Date(),
  };
}
```

---

## Biome Weighting Function

The `getBiomeWeights` function adjusts biome probabilities based on **planet type** and **distance to star**:


| Planet Type | Distance to Star (AU) | Likely Biomes               |
| ----------- | --------------------- | --------------------------- |
| Rocky       | 0.5 - 2               | Desert, Mountain            |
| Rocky       | 2 - 5                 | Forest, Ocean, Mountain     |
| Rocky       | 5 - 10                | Ice, Desert, Volcanic       |
| Gas         | Any                   | Volcanic, Desert (if close) |
| Ice         | Any                   | Ice, Mountain               |
| Oceanic     | 0.8 - 1.5             | Ocean, Forest               |


---

## Example Planets

### Example 1: Rocky Planet in "Sol" System

- **Name**: `Sol-3`
- **Type**: Rocky
- **Distance to Star**: 3.2 AU
- **Biomes**: Desert, Mountain
- **Global Resources**: Iron (1000), Gold (50), Nyx Crystal (1)

### Example 2: Ice Planet in "Andromeda" System

- **Name**: `Andromeda-8`
- **Type**: Ice
- **Distance to Star**: 7.9 AU
- **Biomes**: Ice, Volcanic
- **Global Resources**: Natural Gas (2000), Diamond (10), Frost Shard (1)

---

## Open Questions

1. **Naming Conflicts**: If two planets have the same rounded distance (e.g., `3.2 AU` and `3.4 AU` → both `3`), should we:
  - Add a suffix (e.g., `Sol-3a`, `Sol-3b`)?
  - Recalculate distances to avoid duplicates?
2. **Biome Weighting**: Should the biome probabilities be adjusted further for realism?
3. **Deterministic Generation**: Should planets be generated deterministically (same seed → same planet)?

---

- Finalize biome weighting logic.
- Implement conflict resolution for planet numbering.
- Integrate with MongoDB for persistence.


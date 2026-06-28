# Infinity Galaxy Server Specification: Cube-Based Star System

```yaml
date: 2026-06-08  
author: Roro LeSage  
model: Agent Infinity (Mistral Medium 3.5)  
sources: User specifications
```

## Overview

The galaxy in **Infinity** is dynamically generated and divided into **adjacent cubes** for efficient management and procedural generation. Each cube represents a 3D section of the galaxy, containing a fixed number of stars with unique properties.

---

## Units and Coordinates

- **Distance Unit**: Light-year (LY).
- **Cube Dimensions**: Each cube is a 3D space with sides of **10 light-years** in length.
- **Cube Naming Convention**: Cubes are named based on their integer coordinates (in light-years) relative to a fixed reference point (origin).
  - Example: `cube 10-10-10`, `cube 20-20-20`.

---

## Cube Structure

- **Size**: 10 LY × 10 LY × 10 LY.
- **Star Count**: Each cube contains between **5 and 20 stars**.
- **Star Coordinates**: Stars have fixed coordinates **relative to the cube's origin** (e.g., `(2.5, 7.3, 1.8)` LY within the cube).

---

## Star Naming Convention

- Stars within a cube are named using **Greek letters** followed by the cube's name.
  - Example: `Alpha cube 10-10-10`, `Beta cube 10-10-10`, `Gamma cube 20-20-20`.
- Greek letters are assigned in order of discovery or generation within the cube.

---

## Coordinate System

- **Global Coordinates**: Absolute coordinates in the galaxy (e.g., `(15, 25, 35)` LY).
- **Local Coordinates**: Coordinates relative to the cube's origin (e.g., `(5, 5, 5)` LY within `cube 10-10-10`).
- **Conversion**: 
  - Global = Cube Origin + Local
  - Example: A star at `(2, 3, 1)` in `cube 10-10-10` has global coordinates `(12, 13, 11)` LY.

---

## Data Representation

### Cube Metadata

```markdown
| Field          | Type      | Description                          |
|----------------|-----------|--------------------------------------|
| id             | String    | Unique identifier (e.g., "10-10-10") |
| origin         | [x, y, z] | Global coordinates of cube origin   |
| stars          | Array     | List of star objects in the cube     |
```

### Star Metadata

```markdown
| Field          | Type      | Description                          |
|----------------|-----------|--------------------------------------|
| id             | String    | Greek letter + cube ID (e.g., "Beta 10-10-10") |
| local_coords   | [x, y, z] | Coordinates relative to cube origin |
| cube_id        | String    | ID of the cube the star belongs to   |
| properties     | Object    | Star-specific attributes (e.g., type, mass, temperature) |
```

---

## Procedural Generation

- Cubes are generated **on-demand** as players explore the galaxy.
- Stars within a cube are generated using a **deterministic algorithm** (e.g., seeded noise) to ensure consistency across sessions.
- Star properties (e.g., type, size, resources) are procedurally generated but fixed for a given cube and seed.

---

## Example

### Cube `10-10-10`

- **Origin**: `(10, 10, 10)` LY
- **Stars**:
  - `Alpha cube 10-10-10`: Local `(2.1, 3.4, 5.6)` → Global `(12.1, 13.4, 15.6)`
  - `Beta cube 10-10-10`: Local `(7.8, 1.2, 4.5)` → Global `(17.8, 11.2, 14.5)`
  - `Gamma cube 10-10-10`: Local `(5.0, 8.9, 2.3)` → Global `(15.0, 18.9, 12.3)`

---

## Implementation Notes

- **Storage**: Cube and star data can be stored in **MongoDB** for flexibility.
- **Caching**: Frequently accessed cubes can be cached in **Redis** for performance.
- **API Endpoints**:
  - `GET /cubes/{x}-{y}-{z}`: Retrieve cube data.
  - `GET /stars/{id}`: Retrieve star data by ID.

---

## Next Steps

- Define the procedural generation algorithm for cubes and stars.
- Design the API for cube/star data retrieval and caching.
- Implement the coordinate conversion logic.
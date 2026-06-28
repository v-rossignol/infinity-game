# Infinity - Game and Components Overview

```yaml
date: 2026-06-09
author: Roro LeSage
model: Agent Infinity (Mistral Medium 3.5)
updated: 2026-06-28 by Sonnet 4.6
sources:
  - AGENTS.md
```

**Infinity** is a **multiplayer open-world game** set in a dynamically generated galaxy. Players explore, colonize, and interact in real-time across procedurally generated star systems and planets, harvesting resources, crafting, and competing or collaborating with others.

The project is a **monorepo**: one backend (**Infinity Server**) and multiple independent web clients. REST and Socket.IO share the API prefix `/infinity`.

Local entry: `http://localhost/` or `http://infinity-dev.home.rh/`.

---

## Components

Six logical components; each maps to a sub-project directory when implemented.

### 1. Infinity Server

- **Directory**: `infinity/`
- **Status**: Active
- **Role**: Backend server managing players, real-time synchronization, procedural generation, and data persistence.
- **Stack**: NestJS 11, Socket.IO 4, PostgreSQL 16 (TypeORM), Mongoose 8, Redis 7 *(dev Docker: MongoDB 4.4)*

### 2. Stellar Gate

- **Directory**: `stellar-gate/`
- **Status**: Active
- **Role**: Authentication client for login, registration, and password recovery. *(Backend auth incomplete.)*
- **Stack**: React 18, TypeScript 5, Vite 5, React Router 6, Zustand 4, Axios, MUI 5
- **Dev port**: `3001` — served at `/stellar-gate/`

### 3. Cosmos Governance

- **Directory**: `cosmos-governance/`
- **Status**: Active
- **Role**: Administration client with dedicated sign-in, server health monitoring, and future operational dashboards. *(Backend auth incomplete.)*
- **Stack**: React 18, TypeScript 5, Vite 5, React Router 6, Zustand 4, Axios, MUI 5
- **Dev port**: `3002` — served at `/cosmos-governance/`

### 4. Galaxy View

- **Directory**: `galaxy/` *(directory not created yet)*
- **Status**: Planned
- **Role**: 3D galaxy visualization client for navigating star systems.
- **Purpose**: Allows players to explore the galaxy and select star systems.
- **Stack**: TBD

### 5. Solar System View (`solaris`)

- **Directory**: `solaris/`
- **Status**: Active
- **Role**: 2D star system visualization client.
- **Purpose**: Displays stars, planets, and resources within a star system.
- **Stack**: React 18, TypeScript 5, Vite 5, PixiJS 7, Zustand 4, Axios
- **Dev port**: `3003` — served at `/solaris/` *(Caddy route not wired yet)*

### 6. Planetary View (`terra-view`)

- **Directory**: `terra-view/`
- **Status**: Active
- **Role**: 2D planetary surface visualization client.
- **Purpose**: Renders procedurally generated planetary maps with biomes, resources, and challenges.
- **Stack**: React 18, TypeScript 5, Vite 5, PixiJS 7, Zustand 4, Axios
- **Dev port**: `3000` — served at `/terra-view/`

---

## Client flow

After login, Stellar Gate redirects to Galaxy View. Players then navigate down to a star system and onto a planet surface.

```
Stellar Gate → Galaxy View → Solar System View (Solaris) → Planetary View (Terra View)
                    ↕               ↕                              ↕
                              Infinity Server (REST + Socket.IO)
```

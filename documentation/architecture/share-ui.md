# Shared UI Module Architecture

```yaml
date: 2026-06-28  
author: Roro LeSage  
model: Chat GPT 2.5
```

## Overview

A `shared-ui` module is a great solution when multiple React clients share a significant part of their user interface. Its primary goal is not only to reduce code duplication but also to ensure visual and functional consistency across applications.

For a multiplayer game with multiple clients (e.g. Game Client and Admin Client), a dedicated UI package helps maintain a clean architecture and improves long-term maintainability.

---

# What Should Be Shared?

The `shared-ui` package should contain components and utilities that are independent of any specific client.

Typical contents include:

* Generic UI components

  * Button
  * Modal
  * Tooltip
  * Tabs
  * Spinner

* Reusable game-related components

  * PlayerAvatar
  * InventoryGrid
  * HealthBar
  * ItemIcon
  * ChatMessage

* Theme

  * Colors
  * Typography
  * Spacing
  * Animations

* React hooks

  * useKeyboard
  * useResize
  * useDebounce

* UI utilities

  * Formatters
  * CSS helpers
  * Icons

---

# What Should *Not* Be Shared?

The UI library should remain presentation-focused.

It should **not** depend on:

* Routing
* WebSocket communication
* Global application state
* API calls
* Client-specific business logic

Keeping these concerns outside the library makes it reusable across multiple applications.

---

# Recommended Project Structure

A typical monorepo organization could look like this:

```text
project/
│
├── apps/
│   ├── game-client/
│   └── admin-client/
│
├── packages/
│   └── shared-ui/
│       ├── src/
│       │   ├── components/
│       │   ├── hooks/
│       │   ├── theme/
│       │   ├── icons/
│       │   ├── styles/
│       │   └── index.ts
│       │
│       └── package.json
│
└── package.json
```

Both clients simply import components from the shared package:

```tsx
import { Button } from "@game/shared-ui";
```

instead of using deep relative imports.

---

# Use a Monorepo

For React + Vite projects, a monorepo is usually the most convenient setup.

Popular choices include:

* npm Workspaces
* pnpm Workspaces
* Turborepo
* Nx (for larger projects)

This allows all packages to evolve together while keeping clear boundaries between them.

---

# Keep the Library Independent

One of the most important design rules is to avoid coupling UI components to application state.

Avoid:

```tsx
function Inventory() {
    const inventory = useGameStore();

    ...
}
```

Instead, expose data through props:

```tsx
function Inventory({ items, onUse }) {

}
```

Then let each client provide the implementation:

```tsx
<Inventory
    items={inventory}
    onUse={useItem}
/>
```

This makes the component reusable in any application.

---

# Styling

Several approaches work well.

## CSS Modules

Simple, lightweight, and easy to maintain.

```
Button.module.css
```

## Tailwind CSS

A popular choice for modern React applications.

The shared package mostly exports components while the styling remains highly composable.

## Styled Components / Emotion

Useful when themes are highly dynamic or customizable.

---

# Shared Assets

The package can also expose common assets:

```
icons/
images/
fonts/
```

For example:

```tsx
export { SwordIcon } from "./icons/SwordIcon";
```

---

# Storybook

As the component library grows, Storybook becomes extremely valuable.

Each component can be developed and tested independently.

Example:

```
Button

Modal

InventoryGrid

PlayerCard

HealthBar
```

This allows UI development without launching the full game.

---

# Versioning

If both clients evolve together, workspace dependencies are usually sufficient.

If they become independent applications, version the library separately using Semantic Versioning (SemVer).

Example:

```
shared-ui
    1.2.0

game-client

admin-client
```

---

# Dependency Rules

The shared UI library should avoid depending on application-specific technologies.

Avoid architectures like:

```
shared-ui
    ↓
zustand
    ↓
game-client
```

or

```
shared-ui
    ↓
socket.io
    ↓
server
```

A UI library should never depend directly on:

* Server code
* Networking
* Persistence
* Global application state

---

# Prefer Composition

Instead of creating many specialized components:

```
GameButton
AdminButton
PlayerButton
MonsterButton
```

Prefer configurable components:

```tsx
<Button variant="danger" />
```

or

```tsx
<Button icon={<SwordIcon />} />
```

Composition leads to a simpler and more scalable API.

---

# Stateless Components

Shared components should expose their state through props rather than managing application logic internally.

Example:

```tsx
<Button
    loading
    disabled
    error
/>
```

The component renders the requested state but does not decide when those states occur.

---

# Design Stable APIs

Keep component APIs concise and expressive.

Prefer:

```tsx
<PlayerCard
    player={player}
    onKick={...}
/>
```

instead of exposing dozens of unrelated props:

```tsx
<PlayerCard
    playerName
    playerLevel
    playerXP
    playerGuild
    playerInventory
    ...
/>
```

Passing domain objects generally leads to cleaner and more maintainable interfaces.

---

# Recommended Package Organization

For a multiplayer game, consider splitting shared code into several focused packages instead of one large "shared" package.

```
packages/
│
├── shared-ui/
│   ├── components/
│   ├── theme/
│   ├── icons/
│   └── hooks/
│
├── shared-types/
│   ├── Player.ts
│   ├── Item.ts
│   ├── Packet.ts
│   └── GameEvents.ts
│
├── shared-utils/
│   ├── formatters/
│   ├── math/
│   ├── random/
│   └── helpers/
│
└── shared-config/
    ├── constants.ts
    ├── colors.ts
    └── settings.ts

apps/
├── game-client/
└── admin-client/

server/
```

This separation provides several benefits:

* **shared-ui** focuses exclusively on React presentation components.
* **shared-types** guarantees consistent data models between the server and all clients.
* **shared-utils** centralizes reusable utility functions.
* **shared-config** stores shared constants, colors, timing values, and configuration.

This architecture scales well as the project grows and prevents the UI layer from becoming tightly coupled to the game logic or the server implementation.

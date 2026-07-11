# Infinity API contracts

Cross-project API contracts for the Infinity Server.

**Agent rule — Markdown in `contracts/`:** Do not edit any `.md` file here (including `schemas/README.md`) unless the user explicitly asks or approves first. OpenAPI, AsyncAPI, and JSON Schema files may be updated to match implemented code when that is part of the task.

---
| Path | Purpose |
|------|---------|
| [`auth-api.yaml`](auth-api.yaml) | Auth routes (`/auth/*`) — Stellar Gate |
| [`admin-api.yaml`](admin-api.yaml) | Admin routes (`/admin/*`, JWT + admin role) — Cosmos Governance |
| [`game-api.yaml`](game-api.yaml) | Health, players, galaxy, planets, resources — game clients |
| [`client-terra-view.yaml`](client-terra-view.yaml) | Terra View client integration — REST bootstrap and planned real-time |
| [`client-solaris.yaml`](client-solaris.yaml) | Solaris client integration — landing page bootstrap and planned system map |
| [`openapi-shared.yaml`](openapi-shared.yaml) | Shared security schemes, parameters, and error responses |
| [`asyncapi.yaml`](asyncapi.yaml) | AsyncAPI 3.0 Socket.IO events (implemented handlers) |
| [`game-rules.md`](game-rules.md) | Draft gameplay rules, locations, and client/server authority |
| [`units.md`](units.md) | Unit type catalog — vehicules and buildings seeded from server constants |
| [`schemas/`](schemas/) | JSON Schema (Draft 2020-12) — request DTOs, shared types, and response shapes |

**Source of truth:** implemented controllers and DTOs in [`infinity/src/modules/`](../infinity/src/modules/). When code and OpenAPI / AsyncAPI / JSON Schema diverge, update those contract files to match code unless the task is explicitly to change server behavior. Markdown drafts (`game-rules.md`, `resources.md`, `units.md`) follow the agent rule above.

- OpenAPI REST: [`auth-api.yaml`](auth-api.yaml), [`admin-api.yaml`](admin-api.yaml), [`game-api.yaml`](game-api.yaml) (shared pieces in [`openapi-shared.yaml`](openapi-shared.yaml))
- AsyncAPI: [`asyncapi.yaml`](asyncapi.yaml)
- JSON Schema: [`schemas/README.md`](schemas/README.md)
- Game rules (draft): [`game-rules.md`](game-rules.md)
- Unit catalog (draft): [`units.md`](units.md)


# Infinity DTO schemas

JSON Schema definitions for Infinity Server Data Transfer Objects (DTOs) and closely related response shapes.

```yaml
date: 2026-06-14
author: Roro LeSage
sources:
  - infinity/src/modules/**/dto/
  - infinity/src/shared/interfaces/player-location.interface.ts
  - infinity/src/modules/auth/dto/auth-user.dto.ts
```

## Layout

| Directory | Contents |
|-----------|----------|
| `auth/` | Auth request DTOs and `AuthUser` response |
| `players/` | Player location request DTOs |
| `galaxy/` | Galaxy query DTOs |
| `planets/` | Planet query DTOs |
| `shared/` | Reusable fragments (`Vec2Local`, `PlayerLocation`, …) |
| `responses/` | REST response bodies referenced by OpenAPI specs in [`../`](../) |
| `socket/` | Socket.IO event payloads referenced by [`asyncapi.yaml`](../asyncapi.yaml) |

REST: [`../auth-api.yaml`](../auth-api.yaml), [`../admin-api.yaml`](../admin-api.yaml), [`../game-api.yaml`](../game-api.yaml) (OpenAPI 3.1).
Real-time events: [`../asyncapi.yaml`](../asyncapi.yaml) (AsyncAPI 3.0).

## Conventions

- **Draft:** JSON Schema Draft 2020-12 (`$schema`: `https://json-schema.org/draft/2020-12/schema`).
- **Filenames:** kebab-case matching the DTO name without the `Dto` suffix (e.g. `LoginDto` → `auth/login.json`).
- **`$id`:** `https://infinity/contracts/schemas/<path>` — stable identifier; file paths are relative for `$ref`.
- **`x-infinity-source`:** relative path to the TypeScript DTO or interface in the server repo.
- **`x-infinity-usage`:** `body` (default), `query`, or `response` when the shape is not a JSON request body.

## Validation mapping (class-validator → JSON Schema)

| Decorator | JSON Schema |
|-----------|-------------|
| `@IsString()` | `"type": "string"` |
| `@IsNotEmpty()` | `"minLength": 1` |
| `@MinLength(n)` | `"minLength": n` |
| `@IsEmail()` | `"format": "email"` |
| `@IsOptional()` | field omitted from `required` |
| `@IsUUID()` | `"format": "uuid"` |
| `@IsNumber()` | `"type": "number"` |
| `@IsInt()` | `"type": "integer"` |
| `@ValidateIf` allowing `null` | `"type": ["object", "null"]` or nullable union |

NestJS validates request bodies with `ValidationPipe`. Query DTOs use the same decorators but arrive as query-string parameters.

## Schema index

### Auth (`auth/`)

| Schema | HTTP usage |
|--------|------------|
| `login.json` | `POST /infinity/auth/login` body |
| `register.json` | `POST /infinity/auth/register` body |
| `forgot-password.json` | `POST /infinity/auth/forgot-password` body |
| `auth-user.json` | Auth responses and `GET /infinity/auth/me` |

### Players (`players/`)

| Schema | HTTP usage |
|--------|------------|
| `update-location.json` | `PATCH /infinity/players/me/location` body |
| `enter-star-system.json` | `POST /infinity/players/me/location/enter-system` body |
| `enter-planet.json` | `POST /infinity/players/me/location/enter-planet` body |
| `leave-planet.json` | `POST /infinity/players/me/location/leave-planet` body |
| `leave-star-system.json` | `POST /infinity/players/me/location/leave-system` body |
| `update-planet-hex.json` | `PATCH /infinity/players/me/location/planet` body |
| `build-unit.json` | `POST /infinity/players/me/units/{unitId}/build` body |
| `list-buildable-units-query.json` | `GET /infinity/players/me/units/{unitId}/buildable` query |
| `stop-unit.json` | `POST /infinity/players/me/units/{unitId}/stop-build` (and other stop actions) body |
| `move-unit.json` | `POST /infinity/players/me/units/{unitId}/move` body |
| `extract-unit.json` | `POST /infinity/players/me/units/{unitId}/extract` body |
| `park-unit.json` | `POST /infinity/players/me/units/{unitId}/park` body |

### Galaxy (`galaxy/`)

| Schema | HTTP usage |
|--------|------------|
| `stars-by-cube-query.json` | `GET /infinity/stars?cube_id=` query |
| `system-data.json` | Defined DTO; not yet wired to a route |

### Planets (`planets/`)

| Schema | HTTP usage |
|--------|------------|
| `get-planet-query.json` | `GET /infinity/planets/:planetId?systemId=` query |
| `planet-data.json` | Defined DTO; not yet wired to a route |

### Shared (`shared/`)

Reusable types referenced by player location schemas. See individual files for field lists.

### Responses (`responses/`)

REST response bodies used by the OpenAPI specs. Request DTOs remain under module directories above.

### Socket (`socket/`)

Socket.IO event payloads used by [`asyncapi.yaml`](../asyncapi.yaml).

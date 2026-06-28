# TO-BE-FIXED — Deferred Issues

```yaml
date: 2026-06-12
author: Roro LeSage
model: Composer
sources:
  - AGENTS.md
  - deployment/AGENTS.md
  - stellar-gate/AGENTS.md
  - infinity/AGENTS.md
  - documentation/infinity.md
  - deployment/dev/README.md
  - stellar-gate/documentation/infinity/stellar-gate-api.md
  - infinity/documentation/TO-BE-FIXED.md
  - deployment/dev/caddy/Caddyfile
  - infinity/src/modules/auth/
  - stellar-gate/src/services/authService.ts
```

Cross-project issues accepted for now and scheduled for a later iteration. Add new entries at the bottom. When resolved, strike through the item or move it to a **Resolved** section — do not delete history.

Server-only deferred work also tracked in [infinity/documentation/TO-BE-FIXED.md](../infinity/documentation/TO-BE-FIXED.md).

---

## ~~1. Stellar Gate auth — server does not match client contract~~ *(resolved 2026-06-13)*

**Resolved.** Server cookie auth aligned; Stellar Gate verified end-to-end (API + browser). See [stellar-gate/documentation/archive/auth-alignment-development-plan.md](../stellar-gate/documentation/archive/auth-alignment-development-plan.md) and [infinity/documentation/auth.md](../infinity/documentation/auth.md).

Spawn / `enter-game` cookie support: [infinity/documentation/TO-BE-FIXED.md §1](../infinity/documentation/TO-BE-FIXED.md).

---

## 2. Stellar Gate API doc — partial staleness

**Problem:** ~~[stellar-gate-api.md](../stellar-gate/documentation/infinity/stellar-gate-api.md) endpoint legend and checklist still describe gaps~~ — **Re-audited 2026-06-13** after auth alignment. Checklist and endpoint status updated; one cosmetic item remains (`POST /logout` returns `201`).

**Fix later:** ~~Re-audit the full contract doc after auth module work~~ — done. Keep doc in sync when auth evolves.

---

## 3. Caddy dev proxy — no `/galaxy/` route

**Problem:** After login, Stellar Gate redirects to `/galaxy`, but [deployment/dev/caddy/Caddyfile](../deployment/dev/caddy/Caddyfile) only proxies `/stellar-gate/*`, `/terra-view/*`, and `/infinity/*`.

**Impact:** Full-stack local dev via Caddy returns **404** after login until Galaxy View exists.

**Fix later:** Add `handle /galaxy/*` (and later `/solaris/*`) when those clients are scaffolded.

---

## 4. Galaxy View client — not in repo

**Problem:** Post-auth navigation target `/galaxy` has no client implementation or dev server yet.

**Fix later:** Scaffold `galaxy/` per [documentation/architecture/3clients-analysis.md](architecture/3clients-analysis.md); wire Caddy and session handoff (see [stellar-gate/documentation/to-be-defined.md §1](../stellar-gate/documentation/to-be-defined.md)).

---

## ~~5. Terra View client — empty placeholder~~ *(resolved 2026-06-13)*

**Resolved.** `terra-view/` scaffolded (React 18, Vite 5, PixiJS 7, Zustand 4, Axios); dev port `3000` at `/terra-view/`.

---

## ~~6. `documentation/infinity.md` — stale stack versions~~ *(resolved 2026-06-13)*

**Resolved.** [infinity.md](infinity.md) updated: NestJS 11, Mongoose 8, dev Docker MongoDB 4.4 note.

---

## 7. `infinity/AGENTS.md` — MongoDB dev vs target

**Problem:** The server agent guide data-layer table still says **MongoDB 7** on port 27017. [deployment/dev/docker/docker-compose.yml](../deployment/dev/docker/docker-compose.yml) runs **`mongo:4.4`** for AVX compatibility.

**Impact:** Server-side agents may assume 7.x features or bump the compose image incorrectly.

**Fix later:** Align the data-layer table with root/deployment wording (Mongoose 8; dev Docker MongoDB 4.4).

---

## 8. `deployment/dev/README.md` — out of sync with agent guides

**Problem:** The human runbook uses backslash-only paths, omits the first-time `infinity/.env` copy step, does not list planned `/galaxy/` and `/solaris/` Caddy routes, and does not explain the MongoDB 4.4 pin.

**Impact:** Drift between README and `AGENTS.md` files; first-time setup failures if `.env` is missing.

**Fix later:** Mirror the local dev stack table and Caddy sections from [deployment/AGENTS.md](../deployment/AGENTS.md).

---

## 10. Cosmos Governance — admin auth and authorization not enforced

**Problem:** Cosmos Governance has a dedicated admin login UI but uses the same `/infinity/auth/*` endpoints as Stellar Gate. The server does not set httpOnly cookies, implement `/me` or `/logout`, or check an admin role.

**Impact today:**

- Login may fail client-side (expects `{ user }` in response).
- Any authenticated user could access the admin UI once cookie auth works — no role gate.

**Fix later:**

| Task | Area |
|------|------|
| Complete cookie auth per TO-BE-FIXED §1 | ~~`infinity` auth~~ *(done 2026-06-13)* |
| Add admin role to user model and enforce on admin routes | `infinity` auth |
| Optional: dedicated admin auth endpoints or guards | `infinity` + `cosmos-governance` |

---

## 9. Caddy — Socket.IO / WebSocket through reverse proxy

**Problem:** Caddy proxies REST under `/infinity/*` but **WebSocket upgrade** for Socket.IO through port 80 is not documented or configured. Game clients (Galaxy View and later) may need same-origin WebSocket on the unified dev URL.

**Impact today:** Low — Stellar Gate uses REST only. Real-time clients may work against `localhost:4000` directly but not through Caddy until upgrade rules are added.

**Fix later:** When a client needs same-origin Socket.IO, add Caddy `reverse_proxy` WebSocket support and document whether dev clients should use `:4000` or `/infinity/` for the socket path.

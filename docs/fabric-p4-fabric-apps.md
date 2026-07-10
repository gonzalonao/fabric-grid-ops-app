# P4 — `fabric-grid-ops-app` — Fabric Apps + Rayfin (D16–D18)

Last updated: 2026-07-10 · Part of [[fabric-portfolio-plan]] · Repo: `fabric-grid-ops-app`

**Goal:** an **operational Fabric App** (public preview, Build 2026) closing the loop on
P2: a "grid ops console" where an operator acknowledges/annotates the demand-deviation
alerts from Activator and manages the alert-threshold reference data. Built with
**Rayfin** (open-source SDK/CLI): TypeScript entity decorators → auto-generated Fabric SQL
database + GraphQL API + Entra SSO + hosting, deployed with `rayfin up`. Secondary story:
the build itself is done agentically with Claude Code — Rayfin's intended workflow — and
that workflow gets documented as part of the deliverable.

**Minimum shippable (cut line):** app running on the **local Docker stack** with entities,
RLS and UI, plus a recording — still valid portfolio material (Rayfin is MIT, local dev
needs no cloud) if the region/tenant blocks deployment.

**Deployment target (updated 2026-07-10):** the main trial capacity sits in **North
Europe**, where Fabric App (preview) is **not available**, and Gonzalo is **capacity
admin only** (no Tenant settings access, no Entra user creation) — so a parallel
2nd-user trial is impossible too. **Default: ship the local-Docker cut line above**, and
document the region/tenant constraint in the README (it reads as engineering judgment).
Optional stretch, only if D16–D17 go fast: create a fresh personal Entra tenant (Global
Admin), start a trial there in a Fabric App region, and deploy for the recording —
accepting that cross-tenant integration with the main lakehouse isn't possible. Never
cancel the main trial — same-user restarts are often refused.

## Phase A — Setup & scaffold (D16)

- Region/tenant check done Day 0: Fabric App ✗ in North Europe, no tenant admin → build
  targets the **local Docker stack**; no Fabric SQL DB slot is consumed on the capacity.
- `npm create @microsoft/rayfin@latest -- grid-ops` (no target workspace — local stack
  only; add one only if the fresh-tenant stretch happens) from a
  template; run the full stack locally with Docker; read the generated `rayfin.yml` and
  write up what each part provisions.
- Entities (TypeScript decorators — `@entity()`, `@uuid()`, `@text()`, `@date()`,
  `@role()`):
  - `AlertAck` — alert id/timestamp, deviation %, status (new/acknowledged/dismissed),
    operator note, `user_id`.
  - `ThresholdConfig` — indicator, deviation threshold, effective-from.
  - Row-level rule: authenticated users see/edit only their own acks (`@role` policy) —
    demonstrate RLS-as-code.
- **Done:** local stack serves the generated GraphQL API; schema visible in the local DB.

## Phase B — Build the console (D16–D17)

- Frontend: React + TypeScript strict (BOE RAG conventions apply): alert inbox (list,
  filter by status), ack/dismiss with note, threshold editor. Use the type-safe
  `RayfinClient` for queries/mutations — no hand-rolled fetch.
- Feed it: import P2's fired-alert history (small JSON seed) so the inbox has real data.
- Keep an honest `docs/agentic-build.md` log: prompts, what the agent generated, what was
  hand-corrected — this meta-story is rare, current, and interview-friendly.
- **Done:** full CRUD loop works locally end-to-end under an authenticated user.

## Phase C — Deploy & integrate (D17–D18)

*(The deploy half applies only if the fresh-tenant stretch happens. Otherwise substitute:
export `AlertAck` from the local DB as JSON/CSV, load it into the lakehouse, and do the
same join + report page there — the platform-citizen story survives locally.)*

- `npx rayfin up` → deployed app with Entra SSO; verify child items in the portal (SQL
  DB, static content, auth) and the `*.rayfin.windows.net` endpoints.
- Integration back into the platform: the app's SQL database is queryable in Fabric —
  join `AlertAck` against the Eventhouse alert stream in a notebook: *"which deviations
  were acknowledged, how fast, by whom"* → one small Gold table + report page. This turns
  the app from a demo into a data-platform citizen.
- Evidence pack: deployed-app recording, portal child-items screenshots, README with
  architecture (app ↔ SQL DB ↔ OneLake ↔ lakehouse join), agentic-build log.
- Wiki note: create [[fabric-apps-rayfin]].

## Interview drills

What Rayfin generates from a decorated entity (schema, GraphQL, RLS, client types); where
Fabric Apps beats Power Apps + Automate + SQL stitching, and where it doesn't (complex
transactions, non-Entra auth); Fabric SSO/Entra flow; GraphQL vs REST trade-offs here; how
the app's SQL DB shows up for analytics (and the schema-drift rule: schema changes only
via code + `rayfin up`, never in the portal); item permissions (Run and interact vs Edit).

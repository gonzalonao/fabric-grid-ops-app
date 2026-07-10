# Phase A — Setup & scaffold

**Status:** ⬜ not started
**Days:** D16 (≈ 2026-07-26) · **Plan:** [P4 §Phase A](../fabric-p4-fabric-apps.md) ·
**Requires:** P2 Phase D ✅ (fired-alert history exists as seed data)

## Outcome (done criteria)

- [ ] Rayfin project scaffolded; **local Docker stack serves the generated GraphQL
      API**; schema visible in the local database.
- [ ] Entities `AlertAck` and `ThresholdConfig` defined with decorators, including the
      `@role` row-level policy.
- [ ] `docs/rayfin-anatomy.md` explains what each part of the generated project /
      `rayfin.yml` provisions.
- [ ] TypeScript standard chosen and codified in `../../.claude/rules/typescript.md`
      (workspace rule for new languages).
- [ ] `docs/agentic-build.md` started (first entries logged).

## Decisions (made up front — revisit only with a reason)

| Decision | Choice | Why |
|---|---|---|
| Build target | **Local Docker stack only** — no target workspace at scaffold time | Day-0 finding: Fabric App preview ✗ in North Europe, capacity-admin-only tenant. Local dev needs no cloud (Rayfin is MIT); the cut line is still valid portfolio material |
| Cloud stretch | Only if Phases A–B finish fast on D16–D17: fresh personal Entra tenant + new trial in a Fabric-App region; **never touch/cancel the main trial** | Master-plan risk register; same-user trial restarts are often refused |
| Entities | `AlertAck` (alert id, fired_at, deviation_pct, status new/acknowledged/dismissed, note, user_id) · `ThresholdConfig` (indicator, deviation threshold, effective_from) | Directly closes the P2 loop: acknowledge deviation alerts, manage the thresholds that fire them |
| RLS | `@role('authenticated', …)` policy: users see/edit **only their own acks** (`claims.sub.eq(item.user_id)`) | RLS-as-code is the drill answer; demonstrated with two local users in Phase B |
| Schema-change rule | Schema changes **only** via decorators + CLI apply — never in the DB directly | Mirrors the production rule (portal DB is read-only; drift breaks the app); local dev follows the same discipline so the story is honest |
| Language standard | TypeScript strict; standard chosen at A2 via `AskUserQuestion` and codified in `../../.claude/rules/typescript.md` | Workspace rule: no language without a declared standard |

## Steps

### A1 `[YOU]` Learn first (~1.5 h, timeboxed)

- [ ] Fabric Apps overview — read fully (child services, backend URL paths, the
      portal-DB-is-read-only rule, item permissions):
      `https://learn.microsoft.com/fabric/apps/overview`
- [ ] Programming model + project structure:
      `https://learn.microsoft.com/fabric/apps/programming-model` and
      `https://learn.microsoft.com/fabric/apps/project-structure`
- [ ] Skim the Rayfin repo README (`https://github.com/microsoft/rayfin`) and note
      the local-dev commands the current version actually uses — the CLI is young;
      **record the real commands in this file as you go**.

### A1.5 `[CLAUDE]` 🎓 Understanding check — Rayfin model

- [ ] Quiz (`AskUserQuestion`): what Rayfin generates from a decorated entity
      (schema, GraphQL, RLS, typed client); the three child services `rayfin up`
      would create in Fabric (SQL DB, auth, static content) and what each does; local
      auth (email/password) vs deployed auth (Entra SSO only); why the portal DB is
      read-only and what happens on schema drift.
- [ ] Diagram the target architecture: console UI → RayfinClient → GraphQL →
      SQL DB (local Docker now, Fabric SQL DB in the stretch), with the P2 alert
      feed as the data source.
- [ ] Record weak spots for the Phase C drills.

### A2 `[CLAUDE]` TypeScript standard (blocking gate for app code)

- [ ] Propose a standard via `AskUserQuestion` (suggested default: `strict: true`
      tsconfig with `noUncheckedIndexedAccess`, ESLint flat config +
      `typescript-eslint` strict-type-checked, Prettier defaults, Vitest, no `any`,
      explicit return types on exported functions).
- [ ] Write the agreed standard to `../../.claude/rules/typescript.md` (workspace
      level) and reference it from this repo's `CLAUDE.md`. Commit both.

### A3 `[YOU]` Scaffold + first local run

- [ ] Docker Desktop running (engine verified Day 0: 29.5.2).
- [ ] In the repo parent dir: `npm create @microsoft/rayfin@latest -- grid-ops` —
      choose the minimal/starter template; **no target workspace** (local only).
      Merge the scaffold into this repo (scaffold in place or move contents —
      record what was done; first commit stays clean per `github/CLAUDE.md`).
- [ ] Start the local stack per the template README (record the exact command here —
      expected shape: a `rayfin`/`npm run` dev command that brings up Docker
      containers): ______
- [ ] Verify: local GraphQL endpoint responds (the template README names the URL —
      typically a `/api/graphql` path); screenshot the GraphQL playground/response.
- [ ] Commit the scaffold (`chore(app): scaffold rayfin project` on
      `feature/scaffold`, PR → `develop`).

### A4 `[CLAUDE]` Anatomy write-up

- [ ] Read the generated project end-to-end (`rayfin.yml`, `rayfin/data/`, auth
      config, docker compose pieces) and write `docs/rayfin-anatomy.md`: what each
      file/section provisions locally and what it *would* provision in Fabric —
      the local↔cloud mapping table is the interesting part.
- [ ] Start `docs/agentic-build.md` with entries for A2–A4 (prompt → generated →
      hand-corrected).

### A5 `[CLAUDE]` Entities + RLS

- [ ] On `feature/entities`: replace template entities with (schema per Decisions):

  ```typescript
  @entity()
  @role('authenticated', '*', {
    policy: (claims, item) => claims.sub.eq(item.user_id),
  })
  export class AlertAck {
    @uuid() id!: string;
    @text() alertId!: string;
    @date() firedAt!: Date;
    @number() deviationPct!: number;   // use the actual numeric decorator the SDK exposes
    @text({ min: 1, max: 20 }) status!: string; // new | acknowledged | dismissed
    @text({ optional: true, max: 500 }) note?: string;
    @text() user_id!: string;
  }
  ```

  plus `ThresholdConfig` (`indicator`, `thresholdPct`, `effectiveFrom`; readable by
  all authenticated users, writable per the policy the docs support — record the
  exact rule used).
- [ ] Apply the schema locally (`npx rayfin up db apply` or the local-stack
      equivalent — record actual command: ______).
- [ ] PR → `develop`; log the step in `docs/agentic-build.md`.

### A6 `[YOU]` Verify the generated backend

- [ ] GraphQL playground: run a generated query (list `AlertAck` — empty is fine) and
      one mutation (create a row). Screenshot.
- [ ] Inspect the local DB (container's SQL — connection details from the template):
      both tables exist with the decorated columns. Screenshot.
- [ ] Tick done-criteria, `[CLAUDE]` closes: evidence into `docs/evidence/phase-a/`,
      Status ✅, session log.

## Gotchas & deviations

*(expected suspects: CLI flag drift — the scaffold/local-run commands are the #1 thing
to correct in this file; numeric decorator naming; Docker port collisions; Windows
path/line-ending friction in generated files)*

## Session log

- 2026-07-10 — Guide written during repo prep (P4 planning session). Nothing built yet.

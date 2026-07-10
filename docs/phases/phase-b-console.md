# Phase B — Build the console

**Status:** ⬜ not started
**Days:** D16–D17 · **Plan:** [P4 §Phase B](../fabric-p4-fabric-apps.md) ·
**Requires:** Phase A ✅ (local stack up, entities applied)

## Outcome (done criteria)

- [ ] Full CRUD loop works locally end-to-end under an authenticated user: see inbox →
      acknowledge/dismiss with note → edit thresholds.
- [ ] RLS **demonstrated**: two local users, each sees only their own acks
      (side-by-side screenshot).
- [ ] Inbox seeded with P2's real fired-alert history.
- [ ] `docs/agentic-build.md` current through the whole phase.

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Branch | UI work on the reusable **`frontend`** branch; backend/entity tweaks on `feature/*` | Workspace rule for backend+frontend projects |
| Stack | React + TypeScript strict (A2 standard); state kept simple (server-derived via RayfinClient + local component state — no store library) | Three screens don't justify Redux; restraint reads senior |
| Data access | **Type-safe `RayfinClient` only** — no hand-rolled fetch | The generated typed client *is* the Rayfin value prop being demonstrated |
| Seed data | Import `fabric-energy-lakehouse/streaming/data/alerts_fired.json` (P2 Phase D) via a seed script using the GraphQL mutations | Real fired alerts make the demo honest; the script doubles as API usage evidence |
| UI scope | Three views: alert inbox (list + status filter), ack/dismiss dialog with note, threshold editor. Nothing else | Cut-line discipline: D16–D17 only |

## Steps

### B1 `[CLAUDE]` Frontend skeleton (branch `frontend`)

- [ ] Scaffold the app per the template's frontend layout: routing/layout shell,
      RayfinClient wiring, auth-aware header (current user display, sign-out).
- [ ] Views:
  1. **Alert inbox** — table of `AlertAck` (firedAt, deviationPct, status, note);
     filter chips new/acknowledged/dismissed; sort by firedAt desc.
  2. **Ack/dismiss** — row action opening a dialog: status change + required-on-ack
     note; optimistic update via typed mutation.
  3. **Threshold editor** — list + edit of `ThresholdConfig`, validation (positive
     pct, valid date).
- [ ] House style per `../../.claude/rules/typescript.md`; lint/typecheck clean.
- [ ] Log prompts/corrections in `docs/agentic-build.md` **as they happen**.

### B2 `[CLAUDE]` Seed script

- [ ] `scripts/seed-alerts.ts`: read P2's `alerts_fired.json` (path configurable; the
      file lives in the energy repo — copy it into `data/seed/` here and commit the
      copy: it's evidence, not a secret), map to `AlertAck` create-mutations
      (`status: "new"`, `user_id` = the demo user), idempotent on `alertId`.
- [ ] Run against the local stack; inbox shows the real P2 alerts.

### B3 `[YOU]` Auth + the RLS demo

- [ ] Create **two** local users (email/password — local-dev auth mode per the docs).
- [ ] As user 1: acknowledge two alerts with notes. As user 2: acknowledge a
      different one.
- [ ] Verify each user sees **only their own** acks (the `@role` policy at work) while
      the inbox of unacked alerts behaves per the policy you defined in A5 — capture
      the side-by-side screenshot. If shared visibility of `new` alerts conflicts
      with the owner-only policy, this is the moment you discover it: adjust the
      policy (e.g. owner-only applies to *mutations*, reads allowed for
      authenticated) and record the reasoning here — that nuance is interview gold.

### B4 `[YOU]` End-to-end CRUD pass

- [ ] One full loop on camera-ready data: filter inbox → ack with note → see status
      flip → edit a threshold → refresh → everything persisted. Screenshot each step.
- [ ] `[CLAUDE]` Fix anything that snagged; merge `frontend` → `develop` when green.

### B5 `[CLAUDE]` 🎓 Understanding check — GraphQL & RLS

- [ ] Quiz (`AskUserQuestion`): GraphQL vs REST trade-offs *in this app specifically*
      (typed schema generation, over/under-fetching, mutation shape); where the RLS
      policy is enforced (server-side on every request — why the client can't bypass
      it); what the typed client catches at compile time that fetch wouldn't; the B3
      read-vs-mutation policy nuance.
- [ ] Record weak spots for the Phase C drills.

### B6 `[CLAUDE]` Close + evidence + 📣 capture

- [ ] Evidence into `docs/evidence/phase-b/` (inbox with real P2 alerts, ack dialog,
      RLS side-by-side, threshold editor); tick done-criteria, Status ✅, session log.
- [ ] 📣 **Portfolio (capture, not publish):** record a short local-stack screen clip
      now (inbox → ack → threshold edit) while the data is fresh — this is the core
      demo asset whether or not the cloud stretch happens.

## Gotchas & deviations

*(expected suspects: local auth user management UX; RayfinClient query typing on
optional fields; optimistic-update rollback; policy semantics for read vs write)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.

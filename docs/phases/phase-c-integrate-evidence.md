# Phase C — Integrate & evidence

**Status:** ⬜ not started
**Days:** D17–D18 · **Plan:** [P4 §Phase C](../fabric-p4-fabric-apps.md) ·
**Requires:** Phase B ✅ (console works locally with real seed data)

## Outcome (done criteria)

- [ ] The **platform-citizen story executed**: `AlertAck` data joined against the P2
      alert stream in the lakehouse — "which deviations were acknowledged, how fast,
      by whom" as a small Gold table + report page.
- [ ] Evidence pack complete: recording, screenshots, README with architecture
      diagram, finalized `docs/agentic-build.md`.
- [ ] Wiki note `fabric-apps-rayfin.md` created; 📣 portfolio entry live.
- [ ] *(Stretch only)* App deployed via `rayfin up` on a fresh personal tenant.

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Default path | **Local substitution** (per Day-0 finding): export `AlertAck` from the local DB → load into `lh_energy` → join + report there | The join and the analytics story survive without cloud deployment; the constraint paragraph in the README reads as engineering judgment |
| Stretch gate | Attempt the fresh-tenant deploy **only if** it's ≤ mid-D17 and B is fully closed | D18 is the last full P4 day; evidence beats deployment |
| Join home | Notebook + Gold table live in the **energy repo** (`ws-energy-dev`, `gold.fact_alert_ack`); this repo gets the export script + a pointer | Data gravity: the Eventhouse and Gold layer are there; cross-repo linking documented in both READMEs |
| Ack-latency metric | `ack_latency_min = ack_time − fired_at` per alert, plus who acked | One number that turns an app demo into an ops-analytics insight |

## Steps

### C1 `[CLAUDE]` Export `AlertAck` from the local stack

- [ ] `scripts/export-acks.ts`: pull all `AlertAck` rows via RayfinClient (or the
      local DB directly if simpler — record which), write
      `data/export/alert_acks.json` (committed — it's demo evidence, contains no
      secrets). Include `exported_at`.
- [ ] Log in `docs/agentic-build.md`.

### C2 `[YOU]` + `[CLAUDE]` Load + join in the lakehouse (energy repo)

- [ ] `[YOU]` In `ws-energy-dev`: upload `alert_acks.json` to `lh_energy` →
      `Files/external/`; create empty notebook `nb_alert_ack_join` in folder
      `streaming`, attach `lh_energy`, commit (energy repo).
- [ ] `[CLAUDE]` Hybrid flow in the **energy repo**: write the notebook — read the
      JSON + the Eventhouse-backed `silver.demand_rt` (P2 Phase E shortcut) and/or
      `streaming/data/alerts_fired.json` timeline; produce
      `gold.fact_alert_ack` (alert id, fired_at, deviation_pct, acked_by, ack_time,
      `ack_latency_min`, status); PR → merge there.
- [ ] `[YOU]` Run it; verify the Gold table on the SQL endpoint. Screenshot.
- [ ] `[YOU]` Add one page to `rpt_energy` (or a small new report): ack latency by
      day, open-vs-acked counts, worst unacked deviation. Commit via Source control.

### C3 `[YOU]` *(Stretch — skip without guilt)* Fresh-tenant deploy

- [ ] Only if the C0 gate (Decisions) held: new personal Entra tenant (Global Admin)
      → new Fabric trial in a **Fabric-App-supported region** → enable the tenant
      setting (admin portal → Tenant settings → *Fabric Apps (preview)* → Enabled) →
      workspace → `npx rayfin up` from this repo.
- [ ] Verify child items in the portal (SQL DB, Authentication, Static Content) and
      the `https://<app>-app.rayfin.windows.net/` endpoint; sign in via Entra SSO.
- [ ] Screenshots: portal child items, running app under SSO. Record: this tenant is
      integration-isolated from the main lakehouse (documented limitation).
- [ ] **Never cancel the main trial**; the stretch tenant is disposable, the main one
      is not.

### C4 `[YOU]` + `[CLAUDE]` Evidence pack + README

- [ ] `[YOU]` Recording (60–90 s, silent, captions): inbox with real alerts → ack
      with note → RLS side-by-side flash → threshold edit → the Gold ack-latency
      report page (→ deployed app, if C3 happened).
- [ ] `[CLAUDE]` README overhaul per the portfolio standard: one-liner, badges
      (Fabric Apps preview, Rayfin, React, TypeScript, GraphQL), Mermaid architecture
      (app ↔ SQL DB ↔ OneLake/lakehouse join lane), **the region/tenant constraint
      paragraph** (Day-0 finding → local-first decision), run instructions
      (Docker, seed, dev), links: agentic-build log, evidence, energy-repo join
      notebook, LinkedIn/GitHub.
- [ ] `[CLAUDE]` Final pass on `docs/agentic-build.md`: closing reflection — what the
      agent did well, what needed hands, what that means for Rayfin's
      agentic-workflow pitch.
- [ ] Evidence into `docs/evidence/phase-c/`; compress/link the recording (< 10 MB
      or GitHub release).

### C5 `[CLAUDE]` 🎓 Understanding check + wiki + drills

- [ ] Quiz (`AskUserQuestion`) on the P4 drill list: what Rayfin generates; where
      Fabric Apps beats Power Apps + Automate + SQL stitching and where it doesn't
      (complex transactions, non-Entra auth); the SSO/Entra flow; how the app's SQL
      DB shows up for analytics + the schema-drift rule; Run-and-interact vs Edit
      permissions.
- [ ] Create `wiki/learning/fabric/fabric-apps-rayfin.md`: architecture, local-vs-
      cloud mapping, gotchas from all three phases, written-out drill answers.
      Update the wiki learning index.
- [ ] `[YOU]` Run the drills out loud, cold. Stumbled: ______

### C6 `[CLAUDE]` + `[YOU]` 📣 Portfolio close-out

- [ ] `[CLAUDE]` Create `../../portfolio/astro/src/content/projects/fabric-grid-ops-app.mdx`
      (+ Es mirror): the operational-app-closing-the-loop narrative (P2 alerts →
      console → analytics on acks), the agentic-build meta-story, honest local-first
      framing; embed the recording/clip.
- [ ] `[YOU]` Review, commit, deploy the portfolio site.
- [ ] `[CLAUDE]` Set all P4 phase files + phases README to final status; session
      logs; final commit. **P4 closed — P5 starts from
      `../../fabric-iq-agent/docs/phases/`.** (CV: single realignment after D21.)

## Gotchas & deviations

*(expected suspects: cross-repo choreography (this file drives work in two repos —
keep both session logs updated); fresh-tenant trial refusals; rayfin up region
errors; JSON export encoding)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.

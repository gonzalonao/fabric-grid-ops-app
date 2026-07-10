# Phase execution guides

One file per P4 phase (see [fabric-p4-fabric-apps.md](../fabric-p4-fabric-apps.md)).
Each file is the **single source of truth for that phase**: the exact steps to follow,
who does each one, what has been done, and what went wrong. A fresh Claude session
should be able to resume work from these files alone.

Two P4-specific standing rules:

- **Default target is the local Rayfin Docker stack** (Fabric App preview is
  unavailable in North Europe and there's no tenant admin — Day-0 finding). Cloud
  deployment is a stretch, never assumed.
- **`docs/agentic-build.md` is a live deliverable**: every significant Claude prompt,
  what was generated, and what was hand-corrected gets logged as the build happens —
  this meta-story (agentic development with Rayfin, Rayfin's intended workflow) is
  half the portfolio value.

## Conventions

- **`[YOU]`** — manual steps Gonzalo performs (terminal, browser, Docker, screenshots).
- **`[CLAUDE]`** — steps Claude performs locally (code, repo files, git, docs).
- **📣 Portfolio** — a checkpoint to add/update the portfolio entry at
  `../../portfolio/astro` (`src/content/projects/fabric-grid-ops-app.mdx`).
- **🎓 Learning** — a checkpoint where Claude actively checks understanding (quiz,
  diagram, authoritative sources) before the work builds on a new concept.
- Steps are numbered `A1, A2, …` per phase and ordered — do them top to bottom.
- Checkboxes track progress; Claude keeps them, the status line, and the session log
  updated.
- The `Status` line: `⬜ not started · 🔄 in progress (at step X) · ✅ done`.
- **Session log** at the bottom: one dated line per session; deviations also land under
  *Gotchas & deviations*.
- To resume in a new session, tell Claude:
  *"Read docs/phases/phase-<x>.md — we're at step <n>."*
- Branching: entity/backend work on `feature/*`; UI work on the reusable `frontend`
  branch (workspace rule).

## Files

| Phase | File | Days | Status |
|---|---|---|---|
| A — Setup & scaffold | [phase-a-setup-scaffold.md](phase-a-setup-scaffold.md) | D16 | ⬜ |
| B — Build the console | [phase-b-console.md](phase-b-console.md) | D16–D17 | ⬜ |
| C — Integrate & evidence | [phase-c-integrate-evidence.md](phase-c-integrate-evidence.md) | D17–D18 | ⬜ |

# CLAUDE.md

## Project

Operational Fabric App (Rayfin) alert-ack console (P4). Plan:
`docs/fabric-p4-fabric-apps.md` (master: `fabric-portfolio-plan.md` in the
workspace wiki).

**Execution state lives in `docs/phases/`** — one step-by-step guide per phase with
`[YOU]`/`[CLAUDE]` roles, checkboxes, and a session log. At session start, read the
active phase file; keep its checkboxes, status line, and session log updated as work
progresses (conventions in `docs/phases/README.md`).

**Instructions to Gonzalo are always given in great detail** — exact commands, UI
paths, values to type, and how to verify the result, in the style of the `[YOU]`
steps in `docs/phases/`. This applies to ad-hoc guidance too, not just the phase
guides.

## Branching — develop-flow

- `feature/*` branches off `develop`; PRs merge into `develop`. UI work uses the
  reusable `frontend` branch (workspace rule).
- `develop` → `main` by PR only — `main` is the review gate; wait for explicit
  approval before merging.
- Conventional Commits (`type(scope): summary`); no AI attribution in commits or PRs.

## Rules

- Primary language is TypeScript. No `.claude/rules/typescript.md` exists in the
  workspace yet — prompt to choose a standard before writing app code, then create
  and reference it here (workspace rule for new languages).
- Default build target is the **local Rayfin Docker stack** — Fabric App (preview) is
  unavailable in the capacity's region, so cloud deployment is a stretch goal, not
  assumed. Document the constraint in the README.
- Never commit secrets — `.env` and tokens are gitignored.
- Evidence pack as you go: `docs/` notes, screenshots and a short recording. The trial
  workspace is ephemeral; the repo (plus the local-Docker demo) is the durable
  artifact.

## Standing objectives — portfolio, documentation, learning

These three run alongside every phase. Treat them as first-class deliverables, not
afterthoughts, and act on the checkpoints seeded in the phase guides.

1. **Portfolio.** This project is portfolio evidence. The portfolio site lives at
   `../../portfolio/astro` (Astro — English entries in `src/content/projects/*.mdx`,
   Spanish mirror in `src/content/projectsEs/`; `azure-pipeline.mdx` is the closest
   format precedent). At the **📣 Portfolio** checkpoints, capture flagged assets as
   they happen (Phase B's local demo clip especially) and create the entry
   (`fabric-grid-ops-app.mdx`) at the **Phase C** checkpoint. Proactively flag
   portfolio-worthy moments even between checkpoints.
2. **Reproducible documentation.** Keep `docs/phases/` the truthful, step-by-step build
   journal (conventions in `docs/phases/README.md`): tick checkboxes, keep the status
   line and session log current, and record every deviation. For this project,
   `docs/agentic-build.md` (prompts → generated → hand-corrected) is a headline
   deliverable — log it live, not retroactively.
3. **Learning.** A second purpose is Fabric fluency for Gonzalo's job. At the
   **🎓 Learning** checkpoints, actively verify understanding before building on a new
   concept: quiz with `AskUserQuestion`, draw architecture diagrams (`show_widget` /
   Mermaid), and pull authoritative sources with WebSearch. The P4 plan's interview
   drills are the bar.

**Keep adjacent artifacts current** (reminders live at phase ends): the workspace wiki
(`../../wiki` — `fabric-apps-rayfin` learning note at Phase C) and the master CV
(`../../jobsearch/CVs/Master`) once presentable (single realignment after D21 per the
master plan).

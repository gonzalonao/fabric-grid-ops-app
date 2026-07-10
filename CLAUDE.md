# CLAUDE.md

## Project

Operational Fabric App (Rayfin) alert-ack console (P4). Plan:
`wiki/learning/fabric/fabric-p4-fabric-apps.md` in the workspace wiki (master:
`fabric-portfolio-plan.md`).

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

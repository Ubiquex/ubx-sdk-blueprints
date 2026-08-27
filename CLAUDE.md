# CLAUDE.md — ubx-sdk-blueprints

## What this is

Signed, reusable blueprint packages for `ubx` (UBI-74). Coordinating repo:
`github.com/ubiquex/ubiquex`. Real, known content so far: `ci-platform/` —
an `Ubxfile` + Go blueprint (`ciplatform.go`, `bindings.go`) with its own
`blueprint.lock.json`.

## Git rules

- No git workflow convention for this repo has been recorded by a session
  yet — until the founder confirms otherwise, treat it as PR-only, never
  self-merge, matching every repo in this org except `ubiquex` itself and
  `ubiquex-docs`.
- Before pushing more commits to a branch with an open PR, confirm it is
  STILL open (`gh pr list --state open` or `gh pr view <n>`) — a merged PR's
  branch looks identical to any other from `git status` alone, and a push
  after merge lands nowhere near `main`, silently.
- NO AI attribution anywhere in commits or PR bodies.

## Before touching anything

- Blueprints are SIGNED per UBI-74's own design (`docs/architecture.md` in
  `ubiquex` — the Import concept and blueprint promotion model live there).
  Don't hand-edit a package's own content without understanding what
  re-signing/re-locking (`blueprint.lock.json`) that requires.
- This file is new as of UBI-183 (2026-08-27) — a first-pass `CLAUDE.md`
  written from the repo's own real file listing and description, not from
  any session having actually worked here yet. Update it with real,
  specific conventions once a session does.

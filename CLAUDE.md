# CLAUDE.md — ubx-sdk-blueprints

## What this is

Signed, reusable blueprint packages for `ubx` (UBI-74). Coordinating repo:
`github.com/ubiquex/ubiquex`. Real, known content so far: `ci-platform/` —
an `Ubxfile` + `resources.json` (a pre-resolved `ubx:intent/v1` document,
UBI-224's own format, replacing the old prose `resources:` block UBI-237
migrated off) + the built Go blueprint under `ci-platform/go/` (the
nested-`<lang>/`-directory layout, UBI-74 Slice 4) with its own
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
- Never hand-write or guess a real provider's own attribute names into a
  `resources.json`. Verify against the real published `ubx-sdk-<provider>`
  package first (its own generated Go/TS/Python bindings state the real
  wire names directly) -- UBI-237 found the published `ubx-sdk-aws` had
  moved from `hashicorp/aws` (Terraform-sourced) to a CloudFormation-sourced
  schema between when `ci-platform`'s own Ubxfile was first authored and
  when it was migrated; the attribute names genuinely changed
  (`name` → `repository_name`/`queue_name`/`role_name`,
  `message_retention_seconds` → `message_retention_period`,
  `assume_role_policy` → `assume_role_policy_document`, and the CFN
  `aws_iam_policy` resource attaches to a role directly via its own
  `roles:` list -- there is no `aws_iam_role_policy_attachment` resource
  type in the current schema at all, unlike the old Terraform-shaped
  design). A wrong guess here would have silently produced a real,
  signed blueprint describing attributes that don't exist.

## Real, working commands (confirmed live, UBI-237)

Rebuild after editing `Ubxfile`/`resources.json` (run from the repo's own
built `ubiquex` checkout, not a stale/installed copy -- CLAUDE.md's own
rebuild-before-retest discipline in `ubiquex` applies here too):

```bash
ubx blueprint build ci-platform --lang go
cd ci-platform/go && go mod tidy && go build ./... && go vet ./...
```

Regenerate the manifest after any content change (writes a real tarball
too -- pass a throwaway `-o` path, only `blueprint.lock.json` is meant to
be committed):

```bash
ubx blueprint package ci-platform -o /tmp/throwaway.tar.gz
ubx blueprint verify ci-platform
```

To confirm a rebuilt blueprint actually resolves correctly (not just
compiles), a real calling stack, resolved against the real, live AWS
provider (a pinned CFN snapshot, `[providers.aws] source = "ubiquex/aws"
version = "1.0.0"` in a `.ubx/config`, `UBX_PROVIDER_DYNAMIC_REPO` pointed
at a real local `ubx-provider-dynamic` checkout) is the strongest real
proof -- `ubx resolve --from-code` on that stack. `ubx resolve` never
applies anything (schema-fetch/resolve-only), matching `ubiquex`'s own
CLAUDE.md rule that this is always safe against a real provider.

This file is no longer a first-pass placeholder (UBI-183) -- the section
above reflects a real session's own findings, UBI-237.

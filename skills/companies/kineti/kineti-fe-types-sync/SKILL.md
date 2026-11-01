---
name: kineti-fe-types-sync
description: "Trigger only when the user or another skill explicitly invokes this skill to regenerate backend contract types and adapt a Kineti frontend."
---

# Sync backend types in a Kineti frontend

Run only when explicitly invoked as `$kineti-fe-types-sync` (Codex),
`/kineti-fe-types-sync` (Claude Code), or by another skill. Invocation
authorizes generating current backend contracts, adapting consuming code and
mocks, and verifying the result. Creating or editing this skill is not running it.

Work in the selected frontend's task checkout/worktree. Leave uncommitted changes
for review in Changes. Never commit, push, create/update PRs, publish packages or
deploy. Do not create or modify schedules or publish GitHub compare links.

## Resolve the project and contracts

1. Identify the frontend from the current task and Git remotes; ask only if the
   target is ambiguous. Read `AGENTS.md`, nested instructions, `package.json`,
   generator configuration and CI workflows. Use the existing generator and
   project scripts; do not write a replacement generator or workflow.
2. Check open PRs and remote branches for similar work and report relevant
   overlap. Absence of a PR does not prove no one is working on it. Do not merge
   or modify another person's work. Unless overlap creates a concrete conflict,
   continue with the requested local update.
3. Fetch current default branches of the frontend and its backend sources; record
   all SHAs. Work from a current frontend base without overwriting user changes
   or history. If this cannot be done safely, report the blocker; do not reset,
   discard or silently stash existing work.

Known mappings (all repositories are in `kineti1`; verify current configuration):

| Frontend | Backend inputs | Existing generation |
| --- | --- | --- |
| `drones-frontend` | `drones-backend/docs/openapi.json`, `drones-backend/docs/ws_models.json`, `gdk-backend/docs/openapi.json` | `pnpm run generate-api-types`; OpenAPI and WebSocket generators |
| `gdk-frontend` | `gdk-backend/docs/openapi.json` | `pnpm run generate-api-types`; `openapi-ts.config.ts` |

For another Kineti frontend, derive backend repository identities, schema paths,
output paths and generation commands from its instructions and configuration.
Do not guess based only on similar project names. Ask for missing mappings if
they cannot be established. This skill changes frontend consumers, not BE code.

## Prepare and generate

1. Fetch backend sources into isolated copies using clone or fetch/pull
   `--ff-only`, pinned to the recorded remote default-branch SHAs. Do not change
   the user's backend checkouts. Preserve the sibling-directory layout expected
   by the current generator. If paths collide, use an isolated directory layout
   with the frontend symlinked to its task worktree; verify path resolution and
   that generated frontend files land in that worktree. Do not move the frontend
   changes into a separate temporary clone or rewrite shared generator paths.
2. For every separate authentication or dependency installation, including
   retries, explicitly set `AWS_PROFILE=kineti`. Run
   `AWS_PROFILE=kineti pnpm run preinstall`, then
   `AWS_PROFILE=kineti pnpm install --frozen-lockfile` in the frontend worktree.
   Use the project's pnpm version and registry. Do not rely on `.zshrc` or setup
   environment inheritance, reveal secrets, alter global credentials or bypass
   hooks. Diagnose authentication failures with
   `AWS_PROFILE=kineti aws sts get-caller-identity` rather than assuming expiry.
3. Run the existing complete generation script. Review generated outputs and any
   formatting/lint side effects. Keep the regenerated contracts in the final
   diff; do not edit generated types manually. Do not upgrade dependencies or
   change the lockfile just to regenerate types unless demonstrably required
   and within the requested scope.

## Adapt and verify

Run full `typecheck` with `NX_SKIP_NX_CACHE=true NX_NO_CLOUD=true`. If an earlier
stage prevents later stages from running, run the remaining stages separately
to collect the full result. Inspect contract changes against the schemas and
backend implementation before adapting affected code, mocks and tests. Account
for nullability, required fields, enum values, request/response shapes and runtime
semantics, not only compilation. Follow the selected repository's testing policy.
Update translations using its existing workflow if adaptations change visible text.

Do not use `any`, unjustified casts, weakened types, disabled checks or removed
assertions to hide incompatibilities. Fix upgrade-related failures and rerun the
affected checks. Do not edit backend sources to make the frontend pass.

Before calling the result ready, run full typecheck, lint, formatting, the complete
non-watch CI unit test suite, applicable build targets and any translation or
UI/e2e checks required by the changed behavior and repository instructions.
Discover exact commands per project: drones-frontend uses `test-ci`, whereas
gdk-frontend uses `test:ci`. Include uncommitted changes in affected check scopes;
a committed-head-only range may miss this update. Disable Nx cache/cloud for
verification with `NX_SKIP_NX_CACHE=true NX_NO_CLOUD=true`.

Review the final diff and ensure the generator completes reproducibly from the
recorded backend inputs. Fix accidental unrelated churn without discarding user
work. A failed install or generator is a blocker, not evidence of a breaking BE
contract. Do not present partial generation as complete. If a correctly generated
contract exposes an unresolved upstream issue or another external blocker,
report evidence and mark the result incomplete instead of claiming it is green.

## Result

Respond briefly in Polish: no changes / local changes ready in Changes / blocked
or needs a decision. Include frontend and backend SHAs, generated and adapted
areas, similar PRs, and generation, full typecheck and other check results.
List failed or unrun checks. For remaining TS errors, give the file, message and
related contract change. Type compatibility alone does not prove runtime
compatibility. Finish at the local reviewable result.

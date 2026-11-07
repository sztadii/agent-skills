---
name: kineti-fe-types-sync
description: "Trigger only when the user explicitly invokes this skill to regenerate backend contract types and adapt a Kineti frontend."
---

# Sync backend types in a Kineti frontend

## Scope

Generate current backend contracts, adapt frontend consumers/mocks and verify in
the selected task checkout. Leave changes uncommitted for review in Changes;
do not push, publish PRs/packages/compare links, deploy or change schedules.
Editing this skill does not invoke it.

## Workflow

**Resolve inputs**

Read remotes, repository/nested instructions, package.json, generator configuration
and CI. Use existing generators/scripts, never replacements. Check open PRs and
remote branches for overlap; report it and continue unless a concrete conflict
exists. Preserve others' work; absent PRs do not mean absent work.

Fetch frontend/backend default branches and record SHAs. Use a current frontend
base, preserving user work/history; report blockers instead of resetting,
discarding or silently stashing. Verify these `kineti1` mappings against current
config. Both frontends generate with `pnpm run generate-api-types`; backend
inputs are under each backend's `docs/`:

| Frontend        | Backend inputs                                                          | Generators            |
| --------------- | ----------------------------------------------------------------------- | --------------------- |
| drones-frontend | drones-backend: openapi.json, ws_models.json; gdk-backend: openapi.json | OpenAPI and WebSocket |
| gdk-frontend    | gdk-backend: openapi.json                                               | openapi-ts.config.ts  |

For other frontends, derive repositories, schema/output paths and commands from
config; ask for missing mappings. Do not guess or edit BE code.

**Generate**

- Prepare isolated backend copies pinned to recorded SHAs using clone or
  fetch/pull --ff-only. Preserve user backend checkouts and expected sibling paths.
  On collision, use an isolated layout with a frontend symlink to the task checkout;
  verify resolution/output lands there. Do not move frontend changes into a
  temporary clone or rewrite shared generator paths.
- Set `AWS_PROFILE=kineti` on every authentication/install command and retry.
  Run `AWS_PROFILE=kineti pnpm run preinstall`, then
  `AWS_PROFILE=kineti pnpm install --frozen-lockfile` in the frontend checkout.
  Use project pnpm/registry/hooks, not inherited shell setup or global credential
  changes. Keep secrets private; diagnose auth with
  `AWS_PROFILE=kineti aws sts get-caller-identity`.
- Run complete existing generation; inspect outputs and formatting side effects.
  Keep generated contracts unedited. Change dependencies/locks only when
  demonstrably necessary and in scope.

**Adapt and verify**

Run full typecheck, executing later stages separately if blocked by earlier ones.
Before adapting code, mocks and tests, compare changed contracts with schemas and
backend implementation: nullability, required fields, enums, shapes and runtime
semantics. Update visible-text translations through existing tools. Do not hide
incompatibility with any, unjustified casts, weaker types, disabled checks or
removed assertions. Fix related failures and rerun affected checks.

After installation, generation and adaptations are complete, run independent
checks in parallel. Serialize prerequisites and checks that share mutable
resources; collect every result before fixing failures.

Ready requires full typecheck and the complete non-watch CI unit suite. Run other
lint/format/translation/build checks only if PR CI requires them; behavior changes
alone do not justify extra suites. Discover project commands: drones-frontend uses
`test-ci`, gdk-frontend `test:ci`. Include uncommitted changes in check scope;
use `NX_SKIP_NX_CACHE=true NX_NO_CLOUD=true` for Nx verification.

Never run local/remote E2E or browser suites, including Playwright/Cypress, even
if repository instructions mention them. Inspect aggregate scripts/Nx targets;
select non-E2E targets. Report E2E as intentionally unrun, not passed or blocking.
Do not disable or modify CI.

Inspect the final diff and reproducible generation from pinned inputs. Remove
accidental churn, preserving user work. Install/generator failures are blockers,
not evidence of breaking BE contracts. Report unresolved upstream/external issues
as incomplete with evidence.

## Result

Reply in Polish: no changes / ready locally in Changes / blocked or needs
a decision. Include frontend/backend SHAs, generated/adapted areas, similar PRs,
generation/typecheck/other results and failed/unrun checks. For remaining TS errors,
give file, message and contract change. Types alone cannot prove runtime
compatibility. End at the local reviewable result.

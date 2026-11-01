---
name: kineti-fe-gdk-update
description: "Trigger only when the user or another skill explicitly invokes this skill to update @geodome/gdk in a Kineti frontend consumer project."
---

# Update GDK in a Kineti frontend

Run only when explicitly invoked as `$kineti-fe-gdk-update` (Codex),
`/kineti-fe-gdk-update` (Claude Code), or by another skill.
Invocation authorizes updating `@geodome/gdk` to the latest stable published
version, including necessary code and translation adaptations and verification.
If the caller specifies a version, use that version instead. Do not ask for a
second confirmation just to start. Creating or editing this skill is not running it.

Work only in the Kineti frontend consumer repository selected by the user or the
current task context. This skill never commits, pushes or creates
or updates PRs. Leave uncommitted changes ready for a later PR workflow. Do not create or
modify schedules. The existing update-check scheduler remains a notification-only
workflow; its invocation does not authorize this skill.

## Establish the target

1. Verify the repository identity from Git remotes. Use the current task checkout
   selected for this task; do not update other repositories or unrelated checkouts.
   Confirm it is a Kineti frontend that depends on `@geodome/gdk` (for example,
   `kineti1/drones-frontend`). If the target is ambiguous, ask which repo. If it
   builds/publishes GDK itself rather than consuming it, report that this skill
   does not apply; never add a self-dependency. Read its `AGENTS.md`, applicable
   nested instructions, `package.json`
   and relevant translation guidance before editing.
2. Fetch the current remote default branch and record its SHA. Read its
   `package.json` and, when needed, `pnpm-lock.yaml` to establish the baseline.
   Also inspect the task checkout and existing changes. Work from a current base
   without overwriting user work; if this cannot be done safely, report the
   concrete blocker. Do not silently reset, stash or discard changes.
3. Query the package registry configured by the project for published
   `@geodome/gdk` versions. Select the highest stable SemVer release, excluding
   prereleases; do not rely solely on a GitHub tag or assume the local checkout
   reflects the remote baseline. Resolve the target once for this run rather
   than chasing releases published mid-task. Do not install dependencies just
   to compare versions. Report access failures as failures, not “up to date”.
   If the target is already installed and locked, check whether translations,
   adaptations or verification still need completing before reporting no change.
   Do not downgrade a newer version implicitly.

## Authentication and installation

Explicitly set `AWS_PROFILE=kineti` on each separate AWS/CodeArtifact
authentication and dependency installation/update command, including retries.
Do not rely on `.zshrc` or an earlier setup process. Before installing or updating
dependencies, run `AWS_PROFILE=kineti pnpm run preinstall` in the task checkout.
Use the project's pnpm version and existing registry configuration. Do not reveal
tokens, alter global credentials or bypass install hooks. If authentication fails,
use `AWS_PROFILE=kineti aws sts get-caller-identity` to diagnose and report the
actual blocker without printing secrets.

## Perform the update

1. Update only `@geodome/gdk` to the resolved version with pnpm, updating
   `package.json` and `pnpm-lock.yaml` together. Preserve the project's dependency
   version style and avoid unrelated dependency upgrades or lockfile churn.
2. Inspect the relevant GDK release notes, published package APIs/types and changed
   usages in the selected frontend. Adapt code required by the upgrade, following the
   repository patterns. Do not modify the GDK source repository, weaken types or
   suppress errors to make the update pass.
3. Update translations required by the new GDK and application adaptations in
   every supported locale. Follow the repository's translation workflow: extract
   messages from the updated package/application, sync, fill in translations,
   prettify and verify. Use generated message IDs and preserve placeholders and
   existing translations. Do not merely sync keys and leave missing translations.
4. Complete the verification below, fix upgrade-related failures and repeat the
   affected checks until the final changes pass. Handle breaking APIs, templates,
   runtime behavior and test/mock assumptions as part of the upgrade, rather than
   stopping after changing the dependency version. Review the final diff for
   accidental changes.

## Completion checks

Read the current `.github/workflows/` and repository scripts to identify the
non-E2E checks required for a later PR. Run full typecheck and the complete
non-watch CI unit test suite; run other checks only where required by PR CI.
Include uncommitted files in the check scope; do not use a committed-head-only
affected range that misses this skill's changes.
Do not run E2E or browser/UI test suites (including Playwright or Cypress),
even when repository instructions or CI mention them. Inspect aggregate scripts
and Nx targets before running them; use separate non-E2E targets if a wrapper
would also launch E2E. Do not trigger remote E2E jobs or rerun E2E workflows.
Report E2E as intentionally not run under this workflow; do not treat its omission
as a blocker or claim that E2E passed. Do not disable or modify CI checks.

Use these project checks within that scope:

- Lint, full TypeScript checks (including mocks and Angular templates), formatting
  checks and the complete non-watch CI unit test suite using the repository scripts.
  Discover their exact names and non-watch options in the selected project;
  drones-frontend currently uses `lint`, `typecheck`, `format-check-all` and
  `test-ci`. Do not assume these names exist in every frontend.
- The project's CI translation pipeline. In drones-frontend this currently uses
  `drone-fe:extract-i18n`, `sync-locales`, `prettify-locales` and `verify-locales`;
  discover the corresponding scripts and locale paths in other projects.
  After completing translations, rerun the
  pipeline and confirm it introduces no further locale changes relative to the
  prepared local result; the intended changes remain uncommitted.
- The application production build, including supported locales, only if required
  by PR CI. Use existing project targets and local equivalents of CI build steps;
  do not publish images or deploy anything.

Use `NX_SKIP_NX_CACHE=true NX_NO_CLOUD=true` for Nx checks so cached results cannot
hide incompatibilities. If an earlier stage prevents later typecheck stages from
running, execute the remaining stages separately to collect the full result.
After fixes, rerun the failed checks and any checks invalidated by those fixes.
Do not skip/disable failing tests, relax checks, weaken types or remove assertions
to obtain a passing result. Update tests to verify the new intended behavior.

The update is ready only when all required checks finish successfully on the
final local changes. Persist through fixable upgrade failures. If a failure is
pre-existing, requires a GDK source fix or is blocked by unavailable credentials,
services or infrastructure, report the evidence and mark the update blocked or
incomplete; do not silently expand into unrelated repairs or claim a green PR.

Do not check other people's branches, open PRs, conversation history, Slack or
Jira to infer whether the team wants this update. The user handles coordination.
Keep investigation focused on the requested upgrade.

## Result

Respond briefly in Polish: old and target versions, base SHA, changed files and
necessary adaptations, checks and their actual results, and any remaining blocker.
State whether all completion checks passed and changes are ready locally for
review, listing any failed or unrun checks explicitly. Do not present a partial
installation or failed verification as a completed update, or claim runtime
compatibility from typecheck alone.

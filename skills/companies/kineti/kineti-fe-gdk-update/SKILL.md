---
name: kineti-fe-gdk-update
description: "Trigger only when the user or another skill explicitly invokes this skill to update @geodome/gdk in a Kineti frontend consumer project."
---

# Update GDK in a Kineti frontend

## Scope

Update `@geodome/gdk` to the latest stable release or a requested version, adapting
code and translations and verifying the result. Work only in the selected frontend
consumer checkout; leave changes uncommitted. Do not push, publish PRs, change
schedules or modify GDK source. The notification scheduler does not authorize
updates. Editing this skill does not invoke it.

## Dependencies

No skill dependencies

## Workflow

**Establish target**

- Verify checkout identity and consumer dependency from remotes and package.json;
  reject GDK itself to avoid self-dependency. Ask only about ambiguous targets.
  Read repository/nested instructions and translation guidance.
- Fetch the remote default branch; record SHA and dependency/lock baseline.
  Inspect task changes and use a current base without reset, discard or silent
  stash. Report a blocker if user work cannot be preserved.
- Resolve the highest stable SemVer once from the configured registry, excluding
  prereleases, unless a version was specified. Do not rely on tags, install to
  compare versions, implicitly downgrade or label access failures “up to date”.
  An installed/locked target still requires outstanding translations, adaptations
  and verification.

**Update**

Set `AWS_PROFILE=kineti` on every AWS/CodeArtifact authentication and dependency
install/update command, including retries. Before installation/update, run
`AWS_PROFILE=kineti pnpm run preinstall`. Use project pnpm/registry and hooks;
never expose tokens or change global credentials. Diagnose auth failures with
`AWS_PROFILE=kineti aws sts get-caller-identity`.

- Update only GDK in package.json and pnpm-lock.yaml, preserving version style
  and avoiding unrelated upgrades or lockfile churn.
- Inspect release notes, published APIs/types and consumer usages. Adapt breaking
  APIs, templates, runtime behavior and mocks using repository patterns. Never
  weaken types, suppress errors, disable tests/checks or remove useful assertions.
- Complete translations in every supported locale through the existing extraction,
  sync, translation, prettify and verification pipeline. Extract from the updated
  package/application; preserve generated IDs, placeholders and translations.
  Syncing keys alone is insufficient.

**Verify**

After installation, adaptations and translation generation are complete, run
independent checks in parallel. Serialize prerequisites and checks that share
mutable resources; collect every result before fixing failures.

Read CI workflows/scripts. Run full typecheck and the complete non-watch CI unit
suite; run other non-E2E checks only if PR CI requires them. Include uncommitted
changes in affected scopes. Inspect aggregate scripts/Nx targets before running.
Never run local or remote E2E/browser suites, including Playwright/Cypress, even
if repository instructions mention them. Select non-E2E targets; report E2E as intentionally
unrun, not passed or blocking. Do not modify CI checks.

Discover project commands; drones-frontend examples:
`lint`, `typecheck`, `format-check-all`, `test-ci`; translations use
`drone-fe:extract-i18n`, `sync-locales`, `prettify-locales`, `verify-locales`.
Rerun the translation pipeline; confirm no further locale changes. Build
production/supported locales only if PR CI requires it; never publish images or
deploy.

Use `NX_SKIP_NX_CACHE=true NX_NO_CLOUD=true` for Nx checks. Run later typecheck
stages separately if blocked by earlier ones. Fix upgrade failures; rerun
failed/invalidated checks on final changes and inspect the diff. Update tests
for intended behavior without relaxing validation.

Persist through fixable failures. Pre-existing issues, required GDK source fixes
or unavailable credentials/services/infrastructure leave an incomplete result
with evidence. Do not make unrelated repairs or search others' branches, PRs,
conversations, Slack or Jira for coordination; the user handles it.

## Result

Reply briefly in Polish with old/target versions, base SHA, changed areas,
adaptations, actual check results and blockers, including failed/unrun checks.
Ready requires all required checks passing on final local changes. A partial
install is incomplete; typecheck alone cannot prove runtime compatibility.

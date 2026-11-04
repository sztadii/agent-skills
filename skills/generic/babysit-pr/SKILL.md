---
name: babysit-pr
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Babysit PR

## Scope

Maintain an identified PR through the requested endpoint. Invocation authorizes
in-scope fixes, verification, commits/pushes, evidence-backed replies and resolution
of this workflow's verified threads. Default endpoint: confirmed merge without further
confirmation once the conditions below are met. Honor narrower user or caller
constraints, including “do not merge”. No prior skill run is required.

Do not invoke `$council`.

Closing PRs and dismissing reviews require authority. Prefer update merges on
shared branches; authorized rewrites use a lease tied to the inspected head.

## Dependencies

Required: [$simplify](../simplify/SKILL.md), [$proof](../proof/SKILL.md),
[$file-pr](../file-pr/SKILL.md) and
[$leaving-pr-comments](../leaving-pr-comments/SKILL.md).

Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.

## Workflow

**Maintain**

1. Read current head/base, required checks/reviewers, repository guidance, feedback
   and all unresolved threads, including outdated ones; paginate. Optional bots
   do not create indefinite waiting gates. Record the initial diff baseline before
   editing, as described below.
2. Verify feedback against current code. Apply the complexity gate below before
   editing; fix simple, substantiated in-scope issues and CI causes;
   retry transient failures only with a reason. Update the base or resolve
   conflicts when integration or policy requires it. Explain false positives and
   reasons for deferring unrelated requests.
3. Include caller-prepared changes. Run `$simplify` on the complete local fix diff
   after every code-changing round, even over 100 lines, and whenever the user
   requests it. Simplify before measuring size or handing back fixes. A no-op is valid;
   then invoke `$proof` on the simplified snapshot to verify affected behavior,
   acceptance criteria and required checks. Fix in-scope failures and repeat
   `$simplify` then `$proof` after further code changes; reuse only applicable
   evidence. Failed or incomplete required verification blocks publication and
   merge. Skip both skills for CI checks, replies and metadata-only rounds unless
   the user explicitly requests `$simplify`.
   Inspect the final diff.
4. Publish changes through `$file-pr` with the existing PR, evidence and constraints;
   use its publication result. Do not bypass its blockers or call it without
   changes.
5. Delegate replies and resolution to `$leaving-pr-comments` with handled feedback,
   scope, fix revision/evidence or reasons for no change. Track pending feedback
   using its returned thread state.

**Merge**

A draft awaits human review. Only a human may mark it ready; never perform or
delegate that transition. Continue maintenance while waiting.

Merge requires ready status set by a human, green CI on the current head, valid
repository-required approvals and all review threads resolved, including outdated
ones. Recheck expected head, these conditions and mergeability immediately before
merging. Never bypass branch protection or required reviews. Merge without further
confirmation and verify the actual merge; pending or unavailable requirements leave
the endpoint incomplete.

**Size and complexity**

Before edits, save initial head, diff base and Git added/deleted counts. Initial
size = additions + deletions. After `$simplify`, before publication, measure the
complete PR diff on the same basis: task commits, staged/unstaged/untracked task
files, tests and generated files. Exclude unrelated work and changes solely from
base updates.

Growth = current additions + deletions − initial size. Edited lines count as both
deletion and addition. Measure PR diff growth, not total size or the patch between
heads. Exactly 100 lines is allowed; more exceeds the limit. Preserve the baseline
across rounds, pushes and wakes. Small or negative growth does not prove safety;
assess scope and behavior separately. Never compress code, drop tests or split
batches to evade the limit.

Before editing and whenever scope changes, flag drastic suggestions (broad rewrites,
architectural or major behavior changes) directly to the user and ask how to proceed;
do not implement them. Simplify existing fixes before handback; with no fixes,
run `$simplify` only if requested.

For simple fixes, ask the user how to proceed if cumulative growth after `$simplify`
exceeds 100 lines. Include review links, proposed scope, initial/current counts,
growth and simplification result.

Either gate blocks publication, merge and thread resolution. Keep fixes local,
report the reason and await explicit direction; do not proceed with a smaller
subset on your own. Asking does not require a PR comment.

**Wait and finish**

When immediate actions are done but the endpoint is pending, follow
[PR event waiting](references/watching.md) for changes affecting mergeability.
Persist PR/head, endpoint, initial baseline/counts, growth, handled feedback,
evidence, pending issues and next action. Retain notification URLs, revision and scope;
reuse verified notifications instead of reposting on wakes. Wakes do not widen scope.

## Result

Report final head, checks/reviews, fixes, blockers and watcher state. Default
completion requires confirmed merge; for maintenance-only endpoints, report the
actual outcome and deferred checks. Stop on closure/cancellation. Missing access,
external decisions or repeated failures without new evidence are blockers.

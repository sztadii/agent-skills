---
name: full-babysit-pr
description: "Maintain an existing PR: handle review feedback and CI, publish approved fixes, reply and resolve threads, and merge once requirements are met. Trigger only when the user explicitly invokes this skill."
---

# Full Babysit PR

## Scope

Maintain an identified PR through the requested endpoint. Invocation authorizes
analyzing feedback and CI, preparing and verifying local fixes, evidence-backed
replies, resolution of this workflow's verified threads and, once the conditions
below are met, merge without further confirmation. Every commit and push of code
changes requires the user's explicit approval of those changes. Default endpoint:
confirmed merge. Honor narrower user constraints, including “do not merge”.

Closing PRs and dismissing reviews require authority. Prefer update merges on
shared branches; authorized rewrites use a lease tied to the inspected head.

## Workflow

**Maintain**

1. Read current head/base, required checks/reviewers, repository guidance, feedback
   and all unresolved threads, including outdated ones; paginate. Optional bots
   do not create indefinite waiting gates.
2. Analyze each new comment and CI failure against current code. Explain false
   positives and reasons for deferring unrelated requests.
3. Drastic changes (broad rewrites, architectural or major behavior changes),
   whether requested in a comment or needed for CI: do not implement. Tell the user
   immediately with the link, the request or failure and your assessment, then
   await direction.
4. Other substantiated issues: fix them locally right away, writing the code as
   in [Implementation](references/implementation.md). Update the base or
   resolve conflicts when integration or policy requires it; retry transient CI
   failures only with a reason. Include changes the user prepared for this PR.
5. After every code-changing round, simplify the complete local fix diff as in
   [Simplification](references/simplification.md), then verify the simplified
   snapshot as in [Verification](references/verification.md), covering affected
   behavior and required checks. Fix failures and repeat both. Skip them for
   replies and metadata-only rounds unless the user requests simplification.
6. Once the fixes work and are ready to commit and push, tell the user the
   handled comments or failures with links, what changed, the diff summary and
   evidence. Do not commit or push until the user explicitly approves; apply
   requested adjustments and repeat step 5.
7. After approval, publish to the existing PR as in
   [Publication](references/publication.md). Do not bypass its blockers or
   publish without changes.
8. Reply to handled feedback and resolve threads as in
   [PR comments](references/pr-comments.md), with the fix revision/evidence or
   reasons for no change. The user's approval of each reported fix or decision
   on a drastic change counts as approval for the confirmed prefix. Resolve a
   thread only after publication and only when its fix is trivial and
   indisputable; leave any fix involving judgment open for the reviewer. Track
   pending feedback by thread state.

**Merge**

A draft awaits human review. Only a human may mark it ready; never perform or
delegate that transition. Continue maintenance while waiting.

Merge requires ready status set by a human, green CI on the current head, valid
repository-required approvals, all review threads resolved, including outdated
ones, and no unpublished or unapproved local fixes. Recheck expected head, these
conditions and mergeability immediately before merging. Never bypass branch
protection or required reviews. Merge without further confirmation and verify
the actual merge; pending or unavailable requirements leave the endpoint
incomplete.

**Wait and finish**

When immediate actions are done but the endpoint is pending, follow
[PR event waiting](references/watching.md) for changes affecting mergeability.
Persist PR/head, endpoint, handled feedback, local fixes awaiting approval,
evidence, pending issues and next action. Reuse sent notifications instead of
reposting on wakes. Wakes do not widen scope.

## Result

Report final head, checks/reviews, fixes (published or awaiting approval),
drastic requests awaiting direction, blockers and watcher state. Default
completion requires confirmed merge; for maintenance-only endpoints, report the
actual outcome and deferred checks. Stop on closure/cancellation. Missing access,
external decisions or repeated failures without new evidence are blockers.

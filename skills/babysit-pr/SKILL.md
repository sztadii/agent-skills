---
name: babysit-pr
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Babysit PR

Run only when the user explicitly invokes `$babysit-pr` (Codex) or
`/babysit-pr` (Claude Code), or another skill explicitly instructs you to invoke
this skill.
Mentions, examples and matching tasks alone do not trigger this workflow.

## Required skill check — first action

Required skills: [$simplify](../simplify/SKILL.md), [$file-pr](../file-pr/SKILL.md) and
[$leaving-pr-comments](../leaving-pr-comments/SKILL.md).

At entry, check once through the host's skill discovery that the skills listed
above are installed and their `SKILL.md` files are readable. If any are unavailable,
stop the entire workflow, including the caller, and tell the user which are
missing. Do not skip, substitute or auto-install them. After this check succeeds,
proceed with the workflow below without repeating the availability check.

## Purpose and scope

Take ownership of maintenance for an identified PR. Invocation authorizes
in-scope fixes, local verification, task commits and pushes, evidence-backed
replies and resolution of this workflow's own verified threads. No prior
full-send, council, proof or file-pr run is required.

This workflow is for small maintenance changes. Apply the size and complexity
check below before publishing fixes; it does not invoke council.

Status transitions require an explicit user instruction; a merge request alone
does not authorize promotion to ready.
Merge only when already requested; never bypass protection or required reviews.
Do not close the PR, dismiss reviews or resolve another reviewer's threads without
that authority. Prefer an ordinary update merge for a shared branch; rewrite task
history only when authorized, using a lease tied to the inspected remote head.

## Maintain

1. Read the current head, base, required checks and review requirements, all review
   feedback and unresolved threads, including outdated threads. Paginate results
   as needed. Load applicable repository review guidance. Discover actual required
   checks and reviewers; do not invent mandatory review bots. Optional bot feedback
   is evaluated when available but does not create an indefinite waiting gate.
   Before editing, save the initial PR diff statistics as described below.
2. Test feedback against current source. Fix substantiated issues and regressions
   within the feature's scope. Investigate CI failures; retry a transient failure
   only with a reason, and address source failures directly. Resolve conflicts or
   update the branch when required for integration or repository policy, not
   merely because the base has advanced. Explain false positives and defer
   unrelated requests with reasons instead of silently expanding scope.
3. Include any prepared in-scope changes handed off by the caller. Always invoke
   [$simplify](../simplify/SKILL.md) on the complete local fix diff, then apply the
   size and complexity check below before publication. Run affected
   checks and required validation after edits. Inspect the final diff;
   use an independent review for a risky change when available and appropriate,
   not for every metadata update. Do not push a known failing fix as complete.
   When changes need publication, invoke [$file-pr](../file-pr/SKILL.md) with the
   existing PR URL, prepared changes, current evidence and user constraints.
   File-pr owns commits, pushes, description updates and publication policy.
   Use its verified result; if it returns a blocker, report maintenance as
   incomplete instead of publishing directly. Do not invoke it when there is
   nothing to publish.
4. Invoke [$leaving-pr-comments](../leaving-pr-comments/SKILL.md) to reply with the
   fix revision and evidence, or the reason no change is warranted.
   Use the verified model/host and user login in the prefix
   `🤖 **<model> responding on behalf of <user>**` on its own line. Reuse existing
   threads; inspect remote state before retrying uncertain posts. Resolve only
   authorized threads after evidence and the response are verified. If another
   reviewer's thread needs their response, track it as pending.
5. Re-read state for the latest head. If the request includes merging, merge only
   after its prerequisites and required reviews are satisfied, using the expected
   head and repository merge policy; verify the actual merge result.

## Size and complexity check

Before any maintenance edits, record the initial PR head, diff base, added lines
and deleted lines. Save their sum as the initial PR size. After simplify and before
publication, measure the complete resulting PR diff using the same comparison
basis, including task commits, staged, unstaged and untracked task files. Include
tests and generated changes; exclude unrelated work and changes brought in solely
by a base-branch update. Use Git's added/deleted line counts: an edited line is
counted as a deletion and an addition, not as a separate modification category.

The limit is growth of the PR diff: current added lines + current deleted lines
minus the initial PR size. It is not the total PR size or the number of lines in
the patch between the initial and current heads. Exactly 100 lines of growth is
within the limit; more than 100 exceeds it. For example, an initial +100/-30 gives
130 lines; +141/-30 gives 171, a growth of 41, so no size notification is needed.
A resulting +201/-30 gives 231, a growth of 101, which exceeds the limit. Preserve
the initial baseline across commits, pushes and wakes; never reset it per comment
or round. A small or negative growth does not establish that a rewrite is safe;
assess scope and behavior separately.

Always run simplify once per maintenance round, regardless of diff size, including
when the round has no code changes. A justified no-op is valid; do not force edits
or expand scope when there is nothing worthwhile to simplify. After simplify,
remeasure and reassess the complete fix diff. Do not invoke council. Preserve
behavior and meaningful validation; do not compress code, drop tests or split
fixes into batches to evade the limit.

If the measured growth exceeds 100 lines after simplify, or the fixes remain
drastic in scope or behavior even below that limit, notify the PR author only
after simplify has completed and the diff has been remeasured and reassessed.
Never send this notification before simplify. Send the notification
through [$leaving-pr-comments](../leaving-pr-comments/SKILL.md), linking the relevant
review comments and reporting the proposed scope, initial and current added/deleted
counts, total growth and simplification result. Stop
publication of those fixes and ask the author to inspect the comments and decide
how to proceed. Keep prepared changes local and report maintenance as blocked on
that decision; do not merge or resolve the affected threads. Resume broader work
only with explicit human direction. A smaller, focused result may proceed through
the normal validation and publication steps.

## Wait and finish

When no immediate action remains but CI or reviews are pending, follow
[PR event waiting](references/watching.md). Persist the PR, head, completion
condition, initial diff base/head and added/deleted counts, latest measured growth,
notification URLs and the revision/scope they cover, handled feedback, evidence,
unresolved issues and next action. Reuse a verified notification for the same
revision and scope instead of posting it again on a wake. Resume
from that state on events; a wake is not permission to widen scope.

For maintenance-only requests, finish when required checks and reviews available
in the current PR status are satisfied, relevant feedback is settled and conflicts
are absent.
A verified draft is a successful maintenance endpoint when available checks and
reviews are settled. Report requirements that cannot run in draft status as
deferred until the user changes status, not as passed; do not promote the PR to
enable them. A merge request finishes only after a confirmed merge and cannot
implicitly authorize a ready transition.
Stop on closure or user cancellation. Repeated failures without new evidence,
missing access or an external decision are explicit blockers, not endless retries.
Report final head, checks/reviews, fixes, pending items and actual watcher state.

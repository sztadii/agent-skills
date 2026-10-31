---
name: babysit-pr
description: Maintain an existing PR through CI failures, review feedback and conflicts until it meets the requested completion condition. Use for autonomous PR follow-through without restarting implementation or requiring another skill.
disable-model-invocation: true
---

# Babysit PR

Take ownership of maintenance for an identified PR. Invocation authorizes
in-scope fixes, local verification, task commits and pushes, evidence-backed
replies and resolution of this workflow's own verified threads. No prior
full-send, council, proof or file-pr run is required.

Preserve draft/ready status. Only the user may mark a draft ready, or explicitly
instruct the agent to do so. Passing checks, completed reviews, babysitting and
a merge request alone do not authorize promotion. Preserve a user's manual change
to ready; do not revert it automatically.
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
2. Test feedback against current source. Fix substantiated issues and regressions
   within the feature's scope. Investigate CI failures; retry a transient failure
   only with a reason, and address source failures directly. Resolve conflicts or
   update the branch when required for integration or repository policy, not
   merely because the base has advanced. Explain false positives and defer
   unrelated requests with reasons instead of silently expanding scope.
3. Run affected checks and required validation after edits. Inspect the final diff;
   use an independent review for a risky change when available and appropriate,
   not for every metadata update. Do not push a known failing fix as complete.
   Commit and push verified fixes, satisfy the host's PR-linking rules immediately,
   and refresh evidence and the PR description where the result has changed.
4. Reply with the fix revision and evidence, or the reason no change is warranted.
   Use the verified model/host and user login in the prefix
   `🤖 **<model> responding on behalf of <user>**` on its own line. Reuse existing
   threads; inspect remote state before retrying uncertain posts. Resolve only
   authorized threads after evidence and the response are verified. If another
   reviewer's thread needs their response, track it as pending.
5. Re-read state for the latest head. If the request includes merging, merge only
   after its prerequisites and required reviews are satisfied, using the expected
   head and repository merge policy; verify the actual merge result.

## Wait and finish

When no immediate action remains but CI or reviews are pending, follow
[PR event waiting](references/watching.md). Persist the PR, head, completion
condition, handled feedback, evidence, unresolved issues and next action. Resume
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

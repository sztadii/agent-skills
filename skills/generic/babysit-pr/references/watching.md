# PR event waiting

Use only after immediate maintenance, while the requested endpoint is pending.
Watch for changes affecting mergeability: comments, reviews, thread resolution,
CI, head/base updates, conflicts, draft/ready status, closure or merge. Waiting
does not authorize additional mutations or widen scope.

1. Inspect current PR state; handle actionable failures, comments and conflicts.
   Finish needed updates and pushes before registering a watcher.
2. Retain the PR URL, head, draft/ready status, requested endpoint, required
   checks/reviews, handled feedback, evidence, pending thread IDs and next action
   in the host's task state.
3. Use the host's PR event mechanism. In T3 Code only the owning parent calls
   `watch_pull_request`, then ends its turn; children report to the parent and never
   watch. Follow tool continuation instructions; do not poll with sleep, shell
   loops, Monitor or scheduled jobs.
4. On events, reread PR state and head, handle newly actionable work and reassess merge
   conditions. Do not repeat completed work or assume evidence survives code
   changes. Re-arm the watch while the requested endpoint remains pending.
5. On completion, cancellation, closure or blocked handback, remove the watch
   (`unwatch_pull_request` in T3 Code). Yielding with an active watch is a pending
   continuation, not a completed handback.

Outside T3 Code, use a host-supported event mechanism and its continuation rules.
If unavailable or registration fails, inspect current state, report pending work
and inactive monitoring. Do not promise background work without a working
mechanism or substitute custom polling. Reread the PR when asked to resume.

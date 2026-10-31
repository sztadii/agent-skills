# PR event waiting

Use this reference only when immediate maintenance is finished and the requested
endpoint still depends on CI, reviews or a merge event. Waiting does not authorize
additional mutations or widen the task.

1. Inspect the latest PR state and handle actionable failures, comments and
   conflicts before registering a watcher. Finish needed updates and pushes first.
2. Retain the PR URL, head, requested endpoint, required checks/reviews, handled
   feedback, evidence, pending thread IDs and next action in the host's task state.
3. Use the host's supported PR event mechanism. In T3 Code only the owning parent
   calls `watch_pull_request`, then ends its turn. Delegated children report their
   result to the parent and never watch. Follow any tool-provided continuation
   instructions; do not poll with sleep, shell loops, Monitor or scheduled jobs.
4. On an event, re-read the current head and resume the pending phase. Do not
   repeat completed work or assume old evidence applies after a code change.
   Re-arm the watch if the requested endpoint is still pending.
5. On completion, cancellation, closure or blocked handback, remove the watch
   (`unwatch_pull_request` in T3 Code). Yielding with an active watch is a pending
   continuation, not a completed handback.

Outside T3 Code, use an available host-supported event mechanism with its own
continuation rules. If none is available or registration fails, inspect the
current state, report pending work and state that monitoring is inactive. Do not
promise autonomous background work without a working mechanism or substitute a
custom polling loop. A later user request to resume includes re-reading the PR.

Wait for actual repository requirements. Missing required checks are pending,
not green. Optional bot output and stale reviews do not establish current-head
approval. A green run does not settle a review thread or prove the PR merged.

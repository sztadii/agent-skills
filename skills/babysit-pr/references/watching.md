# PR event waiting

Shared mechanics only; reading this reference does not authorize fixes or merging.

In T3 Code the owning parent calls `watch_pull_request` and ends the turn.
Delegates never watch. One watcher per PR; handle current events before arming.
On wake, resume the caller's pending stage and current head, not the whole task.
No custom polling/sleep loop or “still waiting” messages. Unwatch before handback,
a user question, or successful completion. A watch event is not approval.

Outside T3 use the owning skill's absolute `scripts/pr-wait` path:
`pr-wait <pr> [--repo owner/name] [--until ready|green|any-change]`.
`ready` waits for required checks, Greptile/Macroscope and settled threads;
`green` for checks when neither bot exists; `any-change` for one event, including
review follow-up. “Ready” here does not change GitHub draft status.
Exit 0: condition met/merged; 1: action needed/closed; 2: invalid input.
Use Monitor in Claude Code, otherwise one long command. Resume after each event;
never duplicate an existing watcher. Green CI does not settle open findings.

If watching fails, inspect rate limits and report the limitation; no tight retry
loop or promise of monitoring that is not active. REST can read PR/checks when
GraphQL is exhausted. Stop on task completion, user stop, closure or a required
decision; report unresolved findings and verification gaps.

---
name: jira-status
description: Assign the Jira issue to the user and track its status during implementation, PR review handoff, and merge only when explicitly invoked. Excludes reviewing someone else's PR.
---

# Jira Status

Activate only when the user explicitly invokes `$jira-status` or names this
skill. Keep it active for that task across follow-up turns, including a later
full-merge or full-send request; do not apply it to unrelated tasks. Preserve
the issue key and verified milestone in handoff context. Never change Jira
status when reviewing someone else's PR, even if this skill is mentioned.
Invocation authorizes these status changes and assignment to the requesting user, not other issue edits,
comments, marking a PR ready, or merging.

## Identify the issue and workflow

Use the issue key or URL provided by the user, or an unambiguous reference in
the current task, branch, or PR. If ambiguous, ask for the key while continuing
independent work; do not update every issue mentioned in a PR.

Read the issue, current status, and available transitions using Jira tools.
Use the actual workflow's matching status or clear equivalent, not hardcoded
transition IDs. Ask if no equivalent is clear. Do not assume Done means Ready
for QA without project evidence.

When implementation begins, also set Assignee to the requesting user, even if
the issue is already In Progress or assigned to someone else. Resolve their
Jira account ID from the connector's authenticated-user lookup or an explicitly
confirmed identity; do not guess from the local username or use an automatic
default assignee. If identity is unclear, ask while continuing independent
work. Skip assignment if already correct and re-read to verify any change.
If invoked after implementation began, reconcile assignment then as well.

## Update at milestones

| Verified event | Jira status or project equivalent |
| --- | --- |
| Begin implementation, including investigating the assigned bug | In Progress |
| Change and verification are complete, and the PR is awaiting review | In Review / Code Review |
| The PR delivering the task is confirmed merged | Ready for QA / To Test |

Apply the latest verified milestone; skip an already matching status and do
not regress a later status merely to replay earlier milestones. Resuming
implementation or addressing review findings may return the task from review
to In Progress. A draft PR alone does not establish review readiness; a merge
request or queued merge does not establish completion.

Re-read after each transition to verify success, and before retrying an
uncertain write. If blocked, report the issue key and blocker and continue
independent work. Never invent required values or force unrelated transitions;
stop retrying if the same failure repeats. Briefly report the verified status
or unresolved blocker in the handoff.

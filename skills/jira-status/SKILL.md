---
name: jira-status
description: Trigger this if the user says 'jira-status' or 'jira status', or another skill invokes it.
disable-model-invocation: true
---

# Jira Status

Track the linked issue across this task and follow-ups. Authorizes assignment
to the verified requesting user and milestone transitions only. Never update
Jira while reviewing someone else's PR.

```ts
async function jiraStatus(task, event = latestVerifiedMilestone(task)) {
  if (task.reviewingSomeoneElsesPr) return;
  const issue = await resolveUnambiguousLinkedIssue(task);
  if (!issue && !task.hasIssueReference) return;
  if (!issue) return askForUnambiguousIssue();
  const workflow = await readIssueAndAvailableTransitions(issue);

  if (task.implementationBegun) {
    const user = await resolveVerifiedRequestingUser();
    if (!user) return askForIdentity();
    await assignIfNeeded(issue, user);
  }
  const milestone = await verifyMilestone(task, event);
  const status = milestone.prMergeConfirmed ? "Ready for QA / To Test"
    : milestone.completedVerifiedChangePublished ? "In Review"
    : milestone.implementationBegunOrResumed ? "In Development / In Progress"
    : undefined;

  if (status) await transitionToActualWorkflowEquivalent(issue, status);
  return rereadAndReportVerifiedState(issue);
}
```

Use actual workflow equivalents, never guessed transition IDs, identity or Done
as an assumed QA status. Skip matching state; do not regress merely to replay
milestones. Unfinished drafts remain in development; queued merges are not merged.
Reconcile on resume. If writes fail, re-read before retrying and stop on repeated
failure. Do not create tickets or change unrelated fields to satisfy this skill.

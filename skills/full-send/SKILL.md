---
name: full-send
description: Trigger this if the user says 'full-send' or 'full send', or another skill invokes it.
disable-model-invocation: true
---

# Full Send

Deliver a verified, maintainable change as a draft PR. Compose the linked skills;
do not repeat their internals. Authorizes implementation, local execution, task
commits/pushes, rebasing with `--force-with-lease`, evidence publication and PR
maintenance. No user review checkpoint before draft creation. Do not mark ready
or merge; those need a separate explicit request.

```ts
async function fullSend(task) {
  const scope = await prepareTaskBranch(task); // Actual base; preserve unrelated work.
  await recordImplementationStarted(scope);
  await jiraStatus(scope); // Assign and enter development; skip when no linked issue.
  await implement(scope);
  await updateAgainstTargetAndResolveConflicts(scope);
  await simplify(scope);

  let evidence = await proof(scope);
  let review;
  while (true) {
    if (!evidence.complete && !await userAcceptsSpecificProofGap(evidence)) return;
    review = await council(scope, evidence, review?.rounds);
    if (review.blocked) return reportAndAskForMissingDecision(review);
    if (review.satisfied) break;
    const fixes = await applyJustifiedInScopeFindings(review);
    if (!fixes.changed) return reportUnresolvedDisagreement(review);
    evidence = await proof(scope); // New council round covers the revised diff.
  }

  const publication = await filePr(scope, evidence, review);
  if (publication.blocked) return reportBlocker(publication);
  await jiraStatus(scope); // Published completed draft => In Review.
  return babysitPr(publication.pr); // Resume maintenance on watcher events.
}
```

Load [simplify](../simplify/SKILL.md), [proof](../proof/SKILL.md),
[council](../council/SKILL.md), [file-pr](../file-pr/SKILL.md),
[jira-status](../jira-status/SKILL.md), [babysit-pr](../babysit-pr/SKILL.md)
when called. Retain scope, revision, proof, council rounds and PR across turns;
resume the current stage instead of reimplementing. A specific proof exception
permits a draft with an honest gap, not a claim of complete verification.

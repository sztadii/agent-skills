---
name: full-review
description: Trigger this if the user says 'full-review' or 'full review', or another skill invokes it.
disable-model-invocation: true
---

# Full Review

Review someone else's PR by composing local proof and independent council review.
Do not edit the author's branch, change Jira/PR status or merge. Never submit
GitHub APPROVE or REQUEST_CHANGES. Local setup and execution are authorized.

```ts
async function fullReview(pr) {
  const scope = await isolateExactHeadAndActualBase(pr); // Preserve local work; read repo/discussion.
  const evidence = await proof({ ...scope, readOnly: true });
  const review = await council(scope, evidence);
  if (review.blocked) return reportAndAskForMissingDecision(review);
  const findings = await verifyAndRankFindings(review); // Bugs vs simplifications vs pre-existing issues.
  if (await headChanged(pr, scope)) return fullReview(pr); // Refresh only affected review/proof.
  reportReviewedShaEvidenceFindingsAndLimitations(pr, scope, evidence, findings);

  const comments = await showExactInlineCommentsWithStableIds(findings);
  const selected = await askWhichCommentsToPublish(comments);
  if (!selected.length) return;
  await revalidateAgainstCurrentHead(selected);
  const threads = await publishInlineThreads(selected); // leaving-pr-comments
  return followUpOnAgreedFindings(pr, threads);
}

async function followUpOnAgreedFindings(pr, threads) {
  await inspectRepliesAndLatestHead(pr);
  await verifyClaimedFixesWithProof(threads);
  await replyWithEvidence(threads); // No approval needed for replies within agreed findings.
  await resolveOwnVerifiedThreadsAfterReply(threads);
  if (await ownFindingsRemainOpen(threads) && await prIsOpen(pr)) {
    return watchAndEndTurn(pr); // Resume here on event; never poll.
  }
  await unwatch(pr);
  return reportReviewOutcome(pr); // "Review done" only when agreed findings are settled.
}
```

Load [proof](../proof/SKILL.md), [council](../council/SKILL.md) and
[leaving-pr-comments](../leaving-pr-comments/SKILL.md). For event waiting, load
[watching](../babysit-pr/references/watching.md), not the PR maintenance workflow.
Council checks simplicity
without invoking simplify's edits on another author's work. Continue static
review when execution is blocked; report unverified behavior honestly.
Keep probes separate; report conflicts rather than fixing the author's branch.

Publish only the approved set as resolvable inline threads on valid diff lines;
report findings that cannot be anchored instead of posting top-level comments.
New findings need approval. A claim of “fixed”, outdated diff or green CI alone
is insufficient. Resolve only own threads after verified fix or explained
withdrawal. Parent uses the supported watcher and ends turn; delegates never
watch. Unwatch before handback/question. Stop on user request, closure or a
required decision; preserve pending findings and reviewed SHA across turns.

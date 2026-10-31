---
name: babysit-pr
description: Trigger this if the user says 'babysit-pr' or 'babysit pr', or another skill invokes it.
disable-model-invocation: true
---

# Babysit PR

Maintain the task PR until available CI and review are settled. Authorizes
in-scope fixes, conflict resolution, task pushes and autonomous analysis/replies
to existing PR feedback. Preserve draft/ready status; do not mark ready or merge.

```ts
async function babysitPr(pr) {
  const state = await readHeadBaseChecksReviewsAndAllUnresolvedThreads(pr);
  if (state.closedOrMerged) return unwatchAndReport(pr);
  await evaluateFeedbackAgainstLatestSource(state); // Include old/outdated threads.
  await replyToFalsePositivesWithReasons(state); // leaving-pr-comments

  if (state.hasValidFindings || state.hasCiFailures || state.behindOrConflicting) {
    await recordImplementationResumed(pr);
    await jiraStatus(pr); // Assign and enter development on actual resumption.
    const changes = await fixInScopeIssuesAndResolveAgainstActualTarget(state);
    if (changes.blocked) return unwatchAndReportBlocker(changes);
    if (changes.needSimplification) await simplify(changes);
    const evidence = await proof(changes);
    const review = changes.invalidateCouncil || !state.review
      ? await council(changes, evidence, state.review?.rounds) : state.review;
    if (!proofCompleteOrSpecificExceptionStillApplies(evidence, state) || !review.satisfied) {
      return continueFixesOrReportSpecificBlocker(changes, evidence, review);
    }
    const publication = await filePr(changes, evidence, review);
    if (publication.blocked) return unwatchAndReportBlocker(publication);
    await jiraStatus(pr); // Completed change published => In Review, even while draft.
    await confirmFixesInThreadsThenResolveVerifiedFindings();
  }

  if (await allAvailableChecksAndRequiredReviewsSettled(pr)) return unwatchAndReport(pr);
  if (await needsUserDecisionOrReadyStatus(pr)) return unwatchAndReportBlocker(pr);
  if (inT3Code()) return owningParentWatchesAndEndsTurn(pr);
  return runSkillPrWait(pr); // Resume on event; never poll or use a sleep loop.
}
```

Load [proof](../proof/SKILL.md), [council](../council/SKILL.md),
[simplify](../simplify/SKILL.md), [file-pr](../file-pr/SKILL.md),
[jira-status](../jira-status/SKILL.md),
[leaving-pr-comments](../leaving-pr-comments/SKILL.md) when called.
Exceptions apply only to the explicitly accepted gap; new gaps need a decision.
Behavior changes and substantive conflict resolutions invalidate affected proof
and council; harmless metadata changes need no new council round. Do not push
unverified fixes. Preserve review rounds and the accepted scope across wakes.

Reply with evidence and resolve only after successfully posting the explanation
and verifying the fix or false positive. Never resolve another human's thread
without permission. Read `.macroscope/check-run-agents/*.md` when present.
CodeRabbit is advisory: evaluate findings, never wait for it. Keep fixes scoped;
report infrastructure failures and reject speculative expansion with reasons.

For watcher setup, event resumption and stop rules, load
[watching](references/watching.md). A draft with all available checks settled is
a successful outcome. If checks require ready status, report that limitation
instead of waiting indefinitely. Ask before closing an obsolete PR. Check actual
target drift, not just conflicts; use `--force-with-lease` for a task rebase only
when authorized by the user or caller.

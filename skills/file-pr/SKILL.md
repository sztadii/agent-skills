---
name: file-pr
description: Trigger this if the user says 'file-pr' or 'file pr', or another skill invokes it.
disable-model-invocation: true
---

# File PR

Publish an already prepared change and its evidence as a draft PR, or update the
matching open PR. Authorizes task commits/pushes and publication of inspected
evidence. Does not implement, simplify, perform council review or manufacture
proof. Missing preparation returns a blocker to the caller.

```ts
async function filePr(scope, evidence, review) {
  await inspectBranchRemotesAndAllTaskChanges(scope);
  const existing = await findMatchingOpenPrAndActualBase(scope);
  await ensureTaskBranch(); // Fresh branch if default, detached or already merged.
  const diff = await inspectCompleteMergeBaseDiff(scope);
  if (!diff.hasTaskChanges) return { blocked: "Nothing to file" };
  if (!evidenceMatchesCurrentDiff(evidence)) return { blocked: "Run proof on current changes" };
  if (review && !reviewMatchesCurrentDiff(review)) return { blocked: "Refresh council review" };
  if (requiredEvidenceMissingWithoutExplicitException(evidence)) return { blocked: evidence.gaps };

  const attachments = await prepareInspectedEvidenceForReviewers(evidence);
  if (attachments.blocked && !attachments.acceptedException) return attachments;
  const text = await writeProblemFirstPrText(diff, attachments, review);
  await checkTextAndAttachmentsForPrivateData(text, attachments);
  await stageOnlyTaskChangesInspectAndCommitIfNeeded();
  await pushToVerifiedRemote(); // Immediately follow harness linking rules for an existing PR.
  await confirmRemoteHeadMatchesLocal();
  const pr = existing ? await updatePreservingUserContentAndStatus(existing, text)
    : await createDraftPr(text);
  await linkPrImmediately(pr);
  await verifyAttachmentsAccessibleAndPrUrlBaseHeadStatus(pr);
  return { pr, evidence, review };
}
```

New PRs are always drafts. Never mark ready, merge, force-push after rejection,
or duplicate an open PR. Preserve existing status and unrelated user content.
Ask about ambiguous scope/base. Standalone invocation obtains missing proof via
[proof](../proof/SKILL.md), then retries publication; do not file with a silent gap.

UI interactions require an inspected recording linked in the description;
a local path is not an attachment. If capture/upload fails, return attempts,
concrete blocker and unverified scenario. Only an explicit exception permits
filing with that gap, recorded in both description and handoff. Repair missing
evidence in existing PRs too.

Follow repo title conventions; explain the problem and resulting behavior.
Include supplied council rounds/scores in a `Council review` table in the
description, not a comment. No AI attribution/footer in titles or descriptions.
On GraphQL exhaustion, create via REST with `draft=true`.

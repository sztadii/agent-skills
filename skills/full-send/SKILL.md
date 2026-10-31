---
name: full-send
description: Trigger this if the user says 'full-send' or 'full send', or another skill invokes it.
disable-model-invocation: true
---

# Full Send

An explicit full-send request authorizes implementation, local verification,
independent review, commits and pushes on the task branch, creating a draft PR,
rebasing that branch with `--force-with-lease`, and merge once the gates below
pass. Complete these authorized steps autonomously without a separate
pre-PR approval checkpoint. This does not authorize production changes,
data deletion, unrelated work, or bypassing branch protection or harness rules.

## Prepare and verify

1. **Track Jira when applicable.** If this task has an unambiguous Jira issue,
   activate [jira-status](../jira-status/SKILL.md) as part of this request: assign
   the requesting user's verified account and move to In Development or its
   workflow equivalent when work begins. No linked ticket means skip Jira;
   ambiguity or unavailable access means report the blocker and continue
   independent work. All later Jira steps are conditional on that linked issue.
   When resuming an existing PR, reconcile its latest verified milestone rather
   than replaying development; return to development only if implementation
   actually resumes. Do not create a ticket to satisfy this workflow.
2. **Finish the change.** Identify the target branch and task scope. Work on a
   task branch, preserve unrelated edits, and make the smallest complete fix.
   Before final verification, update against the latest actual target branch
   and resolve any conflicts. Do not assume the target is always main.
3. **Prove it.** Use [proof](../proof/SKILL.md). The request authorizes ordinary
   local setup and test methods needed for this task, including app execution,
   browser automation, simulators, and captures, within harness permissions.
   For changed UI interactions, obtain and inspect a before/after recording
   demonstrating the actual changed scenario; an unrelated passing E2E is not
   a substitute. Include evidence publication in the task PR as part of this
   authorization, after checking captures for secrets and private data.
   Follow repository commands and fixtures. Report blocked verification instead
   of treating source inspection as proof.
4. **Run council review.** Invoke [council](../council/SKILL.md) for independent
   review by three providers. Give its reviewers the checkout, fixed merge base,
   complete diff including untracked task files, scope, intent, proof/check
   results, and relevant repo guidance. Ask for concrete correctness, regression,
   and integration findings with evidence. Reviewers must not edit, publish, or
   delegate further; keep reviewed files stable until they finish. Evaluate
   findings, apply justified fixes within the task, rerun affected proof, and
   follow council's re-review procedure. Retain its rounds, scores, findings,
   and disagreements for the final report and PR description. If council or a
   required provider is unavailable, report the blocker and ask how to proceed;
   do not silently substitute a single reviewer or self-review.

## Draft creation and user inspection

After verification and independent review, create the draft PR and provide its
link, change summary, proof/check results, council results, and remaining gaps
for inspection. Do not stop at a local patch, proposed PR text, or a question
asking whether to create the draft: full-send already authorizes that action.
Reuse an existing PR for the task instead of opening a duplicate.

If the user explicitly requests inspection or approval before publication,
honor that restriction: prepare the complete diff, evidence, and proposed PR
text, then wait for approval. Ask for clarification when scope or a required
decision is genuinely unresolved; ordinary in-scope fixes and rebases only
require refreshed verification, not renewed permission to create the draft.

## File, review, and merge

Use [file-pr](../file-pr/SKILL.md) to create a draft with concise
proof and council results in its description. Attach the inspected visual
evidence or include a reviewer-accessible link. A local path is not a published
attachment. Before handoff, verify the recording is linked in the PR description
and accessible to reviewers. On resumption, repair missing evidence in an
existing task PR rather than only reporting test results again. If capture or
upload is blocked, state the attempted method, concrete reason, and missing
scenario in the PR and handoff; visual proof remains incomplete until resolved.

Creating a draft and marking it ready are separate actions: full send alone does not authorize ready status.
Honor an explicit ready request for this PR, including one supplied with the initial request;
otherwise finish work possible in draft and hand off for that decision.

Use [babysit-pr](../babysit-pr/SKILL.md) to handle CI and review. Verify fixes
before pushing, and refresh proof after meaningful rebases or conflict fixes.
Immediately after creating or updating the PR with the completed, verified
change, use jira-status to move the ticket to In Review or its equivalent and
re-read the issue to confirm. This applies to draft PRs too: do not wait for
ready status, CI completion, or another user message. If resuming a published
PR, reconcile this milestone before handing back. An early work-in-progress
draft with unfinished implementation does not reach this milestone.
Follow babysit-pr's environment-specific waiting instructions: in T3 Code,
arm the watcher and end the turn; elsewhere use its supported wait command.
Do not create a polling loop. Unwatch before handing control back or asking
for user input.

Merge only when all of these hold on the current head:

- Proof covers the final changes and required checks are green.
- Council review covers the final behavior diff, substantive findings are
  resolved, required approvals are present, and no human review still requests
  changes.
- Relevant review bots have finished according to repository/babysit rules;
  findings, including older unresolved comments, are fixed or dismissed with
  evidence. Do not bypass a required human approval.
- The PR is ready, up to date with its actual target branch, and mergeable.

Fetch and recheck head, base, checks, and reviews immediately before merging;
changes invalidate affected checks or proof. Use the repository's allowed merge
method without bypasses. For queues or auto-merge, wait for confirmed merge
through the supported watcher. Only after confirmed merge, use jira-status to
move the issue to Ready for QA / To Test or a verified equivalent, not assumed
Done. Report the PR link, final head or merge commit, proof, Jira outcome, and
remaining blockers. If a gate cannot pass, complete independent work and ask
for the specific missing decision; leave the PR unmerged.

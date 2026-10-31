---
name: full-send
description: Trigger this if the user says 'full-send' or 'full send', or another skill invokes it.
disable-model-invocation: true
---

# Full Send

An explicit full-send request authorizes implementation, local verification,
independent review, commits and pushes on the task branch, rebasing that branch
with `--force-with-lease`, and merge once the gates below pass. Prepare the work
autonomously, but let the user inspect and amend it before creating a new PR.
Do not assume the user is absent. This does not authorize production changes,
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
   Follow repository commands and fixtures. Report blocked verification instead
   of treating source inspection as proof.
4. **Get independent feedback.** Have a read-only reviewer inspect the complete
   diff, including untracked task files, against a fixed merge base. Provide the
   checkout, scope, intent, checks, and relevant repo guidance. Ask for concrete
   correctness, regression, and integration findings with evidence; the reviewer
   must not edit, publish, or delegate further. Use available delegation under
   the harness rules, without requiring a specific model or another review skill.
   Keep reviewed files stable while the reviewer works. Evaluate findings
   yourself, fix real issues, and rerun affected proof. Request
   focused re-review for substantive risky fixes; report unresolved disagreement
   or unavailable independent review before asking the user to proceed.

## User review before creating a PR

Before creating any new PR, present the completed diff (with a way to inspect
it), change summary, proof/check results, reviewer findings, remaining gaps,
and proposed PR title and description. Ask the user to inspect and approve
creation. Explain that this checkpoint is required by this skill. Do not create
even a draft before that approval. Silence or a request for corrections is not
approval. Apply requested corrections, refresh affected verification, and show
the updated result before asking again. Reuse explicit approval of the current
reviewed result; do not repeatedly ask for the same action. If behavior or scope
changes after approval (including during conflict resolution), refresh proof
and show the revised diff for approval before creation. A clean rebase with
unchanged behavior does not by itself require another approval.

Use the harness's supported approval/question mechanism, or a final question
if none is appropriate. Unwatch any owned PR before a handoff for user input.
An existing PR does not need approval to be created again: inspect and update
it within the user's scope, without opening a duplicate.

## File, review, and merge

After approval, use [file-pr](../file-pr/SKILL.md) to create a draft with concise
proof in its description. Creating a draft and marking it ready are separate
actions: full send alone does not authorize ready status. Honor an explicit
ready request for this PR, including one supplied with creation approval;
otherwise finish work possible in draft and hand off for that decision.

Use [babysit-pr](../babysit-pr/SKILL.md) to handle CI and review. Verify fixes
before pushing, and refresh proof after meaningful rebases or conflict fixes.
When the completed PR is actually handed off for review, use jira-status to
move the ticket to In Review or its equivalent; a draft alone is insufficient.
Follow babysit-pr's environment-specific waiting instructions: in T3 Code,
arm the watcher and end the turn; elsewhere use its supported wait command.
Do not create a polling loop. Unwatch before handing control back or asking
for user input.

Merge only when all of these hold on the current head:

- Proof covers the final changes and required checks are green.
- Independent review covers the final behavior diff, substantive findings are
  resolved, required
  approvals are present, and no human review still requests changes.
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

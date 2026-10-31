# My global skills

Install all skills globally:

```sh
npx skills add sztadii/agent-skills -g --skill '*'
```

Or install one: `npx skills add sztadii/agent-skills -g --skill full-send`.
Invoke a skill by name, `$skill-name`, or from another skill. Load its `SKILL.md`
when called. Existing explicit-invocation settings are preserved.

## Responsibilities

| Skill | Responsibility | Result |
| --- | --- | --- |
| `simplify` | Simplify task code without changing behavior | Smaller, clearer task diff |
| `proof` | Exercise actual behavior and inspect evidence | Revision, checks, suitable evidence and gaps |
| `council` | Independent read-only review by three providers | Verdict, findings, rounds and scores |
| `file-pr` | Publish prepared changes and evidence | New draft or updated existing PR |
| `jira-status` | Reconcile linked issue assignment and milestone | Verified Jira state |
| `leaving-pr-comments` | Format and verify authorized PR comments | Posted comment/thread reference |
| `babysit-pr` | Maintain the task PR through feedback, conflicts and CI | Verified fixes or an explicit blocker |
| `full-send` | Compose implementation, simplification, proof, review and PR maintenance | Verified draft PR |
| `full-review` | Compose proof, council and agreed review follow-up | Evidence-backed review of another author's PR |

Workflow functions are TypeScript-style pseudocode, not executable APIs. Linked
skills own their details. Preserve task scope and current revision across turns;
refresh affected proof and council after substantive changes.

## Deliver a feature

> Implement APP-142: prevent duplicate form submissions. Full send.

`full-send` assigns the linked Jira issue, implements and simplifies the change,
verifies behavior with `proof`, and obtains independent `council` review. The
caller applies justified findings and repeats affected verification/review.
Once council is satisfied, `file-pr` publishes the draft without a user review
checkpoint. `babysit-pr` then autonomously handles CI failures, comments and
conflicts, refreshing evidence and review before publishing fixes.

Proof uses evidence suited to the change: screenshots, recordings or commands
and observed output. Recordings and before/after comparisons are optional;
when capture is unavailable, use an adequate alternative. Attach or link visual
evidence for reviewers. Label simulated dependencies and report their limits.
A real verification gap needs explicit acceptance before filing a PR; a missing
recording alone is not a gap.

The PR stays draft. Available checks settling does not authorize ready status or
merge. If a check requires ready status, report that limitation. Linked Jira
moves to In Review when the completed change is published, including in a draft.
A later, separately requested and confirmed merge can move it to Ready for QA.

## Review another author's PR

> Full review of PR #91. Verify the export flow and report bugs or simplifications.

`full-review` uses local `proof` and independent `council` without editing the
author's branch. It shows proposed inline comments for approval, publishes only
the selected set, then autonomously verifies fixes and replies in those threads.
It does not submit GitHub APPROVE, change Jira or merge.

## Use a focused skill

> Simplify this diff while preserving behavior and useful regression coverage.

> Use proof to reproduce this CLI bug and verify the fix with the actual command.

> File-pr for the prepared pagination fix, including its inspected evidence.

> Babysit PR #84: address valid feedback and CI failures, keep it draft.

In T3 Code the owning parent uses the PR watcher and ends the turn; delegated
reviewers never watch. No custom polling loops. Stop watching before handback or
a user decision. Report unavailable providers, missing proof or infrastructure
blockers rather than claiming complete verification.

---
name: full-pr-review
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Full PR Review

## Scope

Own review and event-based monitoring of the PR inferred from context until the
user approves it, it merges or closes. Every public message needs approval of its
exact text. Never reject the PR or submit REQUEST_CHANGES. Submit APPROVE only on
explicit instruction and under the conditions below.

Review bugs, regressions, unnecessary complexity, excessive tests and unused
additions. Preserve author and unrelated work; isolate probes. Do not fix code,
push, merge or invoke `$babysit-pr`; the author owns delivery. No prior skill run
is required. Explicitly report-only PR reviews end with their report, without
monitoring. This workflow requires a PR, not a standalone diff or commit range.

## Dependencies

Required: [$proof](../proof/SKILL.md), [$council](../council/SKILL.md) and
[$leaving-pr-comments](../leaving-pr-comments/SKILL.md).

Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.

## Workflow

**Review round**

1. Identify the PR from context; ask if missing or ambiguous. Read intent,
   repository rules, discussion and current reviews; check stopping conditions
   first. Before every round, fetch its branch and base, update the
   review checkout to the remote head; verify and record its SHA. Review the complete
   PR diff and test that revision. Use a separate review checkout when needed to
   preserve local work; exclude unpublished changes from the PR verdict.
2. Invoke `$proof` on that snapshot, acceptance criteria and required checks.
   Reproduce suspected failures without fixing code. If execution is blocked,
   continue static review and record gaps.
3. Invoke `$council` on the same snapshot with intent, scope, repository rules,
   `$proof` evidence, the quality checks below and user-selected reviewers/settings.
   It owns independent review and finding validation; do not simplify code or
   launch separate reviewers.
4. Combine both results; validate and deduplicate findings. Assess correctness,
   regressions, integration, maintainability and tests; separate introduced defects
   from existing issues and material findings from style preferences.

Every round also checks unnecessary abstractions, indirection, speculative
extensibility and duplication. Assess tests for distinct behavioral/regression
coverage versus repeated assertions, implementation coupling and disproportionate
maintenance cost; test count alone never justifies cuts. Trace new files, helpers
and dependencies to consumers, including framework discovery, configuration and
runtime loading; missing imports alone do not prove disuse.

Report substantiated quality issues even with working code and passing tests.
Explain their concrete cost or redundancy and a simpler alternative preserving
behavior and useful coverage. Handle them like bugs throughout this workflow.

**Identify and publish**

Allocate permanent finding IDs per PR from a saved increasing counter: `FR-001`.
Keep them across rounds, code movements, rejection, resolution and reopening;
never renumber or reuse them. Replies get separate IDs under their finding,
such as `FR-001-R01`; summaries use IDs such as `FR-S001`.

Show each proposed message's ID, text version (`FR-001 v1`), exact text, location,
reviewed SHA and evidence. Follow `$leaving-pr-comments` formatting without invoking
it yet. Version every text change and retain earlier versions and decisions.
Bind approval/rejection to the PR, message ID and displayed version, never list
position or a later version. Accept shorthand such as "approve FR-001" only when
the intended version is unambiguous; otherwise clarify. Comment approval never
authorizes an APPROVE review.

Ask for all, selected or no comments to be published, even if publication was
requested initially. Never publish rejected or unanswered drafts; do not repeat
rejected drafts without materially new evidence. New findings and substantive
rewrites need fresh approval. These rules cover replies, summaries and helpers.

Before publishing, reread head and refresh affected findings. Send only approved,
still-relevant text through `$leaving-pr-comments`, with scope, revision and text
approval evidence; use COMMENT reviews. Verify published URLs and thread state.
Resolve only this workflow's clearly settled threads under that skill's rules.

**Continue**

Persist PR identity, base/head, verification/coverage, ID counters, finding/message
IDs, versions and decisions, published URL/thread mappings and next actions.
Track findings as pending, published, fixed, withdrawn or explicitly accepted by
the user as nonessential. Rejection of publication does not mean fixed or accepted.
Restore state before assigning IDs or interpreting decisions; never guess missing
mappings. Preserve it across rounds and resumptions.

Arm the host's PR event watcher, then yield, including when awaiting comment
approval or finding no issues. In T3 only the parent watches. Never poll.
New code/base changes trigger `$proof` and `$council` on the current complete PR,
considering earlier findings. Reuse only applicable evidence, rerun affected checks
and disclose coverage gaps if head moves mid-review. Verify findings on replies;
CI results or new evidence trigger affected verification and review as needed.
Do not repeat unchanged rounds without new evidence.

Declare the reviewed SHA ready for human approval only with complete verification
and review coverage, all material findings fixed, withdrawn with evidence or
explicitly accepted as nonessential. List accepted risks separately; otherwise
report remaining findings/gaps. Keep watching regardless of round completion,
rejected comments or a clear result.

A demonstrated, explicitly accepted finding may explain a failed `$proof` check.
Record its ID, check, failure, tested SHA and acceptance as a known exception;
keep `failed`, never claim `passed` or overall `$proof` success. If all other
required coverage is complete, report readiness with exceptions; it may permit
explicitly authorized APPROVE. Unrelated failures, unexecuted checks and missing
review evidence remain blockers.

**Approve and stop**

Require an explicit instruction to approve the PR on the user's behalf; publication
permission, rejected drafts, silence, green CI or a clear review do not suffice.
Reread head and require complete `$proof`/`$council` coverage with no unaccepted
material findings. If head changed after authorization, review it and obtain renewed
authorization. Explain blockers without REQUEST_CHANGES. Verify the submitted
APPROVE and revision.

Unwatch only on verified user APPROVE (direct or explicitly authorized agent
submission), merge, closure or explicit cancellation. Verify user identity and
effective review state; another reviewer's or a historical, dismissed approval
does not qualify.

Blocked tests, reviewers, access or approval capabilities do not end monitoring.
Report gaps, preserve state, keep a functioning watcher and resume affected work
when changes/evidence permit; incomplete required coverage blocks APPROVE.
If monitoring itself fails or is unavailable, disclose that it is inactive;
never claim continued monitoring or completion.

## Result

Report revision, `$proof`/`$council` evidence and coverage, findings/accepted risks,
pending comment decisions, published URLs, thread and watcher state. Distinguish
round completion from a verified workflow endpoint; unresolved findings stay
pending. Explain blocked/cancelled outcomes without invented success or repeated
unanswered questions.

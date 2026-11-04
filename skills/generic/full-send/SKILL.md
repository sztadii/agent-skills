---
name: full-send
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Full Send

## Scope

Own delivery through the user's endpoint or the default of `$babysit-pr`.
Invocation authorizes implementation, local setup, verification, independent
review, task commits/pushes, PR publication and maintenance through `$babysit-pr`.
Deploy only when requested; never infer production deployment.

Honor narrower constraints such as “do not push”. Resolve routine choices without
phase approvals. Escalate only blocking product decisions, access problems or
out-of-scope actions, after finishing independent work.

All commits and pushes must use commit-push, normally through file-pr, including
after repairs. Do not run separate commit/push commands outside that workflow.

## Dependencies

Required: [$proof](../proof/SKILL.md), [$council](../council/SKILL.md),
[$simplify](../simplify/SKILL.md), [$commit-push](../commit-push/SKILL.md),
[$file-pr](../file-pr/SKILL.md),
[$babysit-pr](../babysit-pr/SKILL.md) and
[$leaving-pr-comments](../leaving-pr-comments/SKILL.md).

Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.

## Workflow

**Deliver**

1. Infer acceptance criteria, scope, endpoint, actual base and destination from
   context and repository instructions. Reuse the matching task branch/PR;
   preserve unrelated changes. Do not require tickets or update issue trackers.
2. Implement the complete change, meaningful regression coverage and required
   documentation. Inspect the whole diff for readability and incidental churn;
   keep tests proportional to risk. Avoid redesigns.
3. Invoke `$simplify` once per code review round on the complete local task diff.
   Retain its changes and verification result; a no-op is valid. Stop on failed
   or incomplete verification.
4. Invoke `$proof` on acceptance criteria and required checks. Inspect suitable
   evidence, fix in-scope causes of failures and rerun affected verification.
   Mocks and screenshots establish only exercised behavior; missing optional
   artifacts do not block adequate evidence.
5. Invoke `$council` with the simplified snapshot, intent, evidence and user-selected
   reviewers or settings. Use its validated findings and coverage; fix substantiated
   issues. After substantive changes, rerun `$simplify`, affected checks through
   `$proof` and `$council` on the new snapshot. Cosmetic edits need not restart
   everything. Review gaps leave delivery incomplete; stop rounds without new
   evidence or progress.
6. Invoke `$file-pr` with prepared changes, matching PR, constraints, current
   verification, findings and useful evidence; use its publication result.
7. Invoke `$babysit-pr` with the PR, prepared unpublished changes, evidence and user
   constraints, including any specified endpoint. Follow its maintenance, waiting
   and completion workflow. Verify the target environment for requested deployments.

**Continue**

Pass scope, permissions, revision, evidence and endpoint between skills. Preserve
acceptance criteria and each skill's returned state, evidence, blockers and next
action; do not bypass blockers or repeat child actions. A phase ending does not
complete delivery.

## Result

Report delivered behavior, evidence, PR URL and actual publication/maintenance
state. Distinguish deferred, failed and pending requirements; completion requires
a confirmed endpoint. Do not bypass approvals, invent success or repeat unanswered
questions and unsuccessful attempts without new evidence.

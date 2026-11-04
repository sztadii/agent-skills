---
name: file-pr
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# File PR

## Scope

Publish prepared changes as a new or updated PR. Invocation authorizes in-scope
commits, pushes and publication of inspected evidence. No separate review
approval is required. Do not implement features, repair product code, perform
broad cleanup, merge or maintain CI indefinitely.

## Dependencies

Required: [$commit-push](../commit-push/SKILL.md).

Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.

## Workflow

**Prepare**

- Identify task files, actual base, branch, remote and matching open PR. Inspect
  staged, unstaged and untracked changes; commit no unrelated work. Reuse the
  matching PR and head branch. Otherwise create a task branch when on the default
  branch, detached HEAD or a merged branch. Infer targets from context and
  conventions; ask only about remaining competing targets. Stop before remote
  changes if creating a draft conflicts with the request.
- Inspect the full diff. Reuse applicable verification or run affected checks and
  repository-required validation. Stale reviews do not approve new changes.
- Disclose failed required checks or unverified behavior in drafts. For ready
  PRs, report blockers without pushing unverified changes or changing status.
  If publication requires passing verification, stop before remote changes;
  do not ask the user to waive routine verification.

**PR text**

Follow user and repository requirements, contribution guidance and applicable PR
templates. Infer missing title, language, structure and detail conventions from
a few recent merged PRs; use concise problem-and-result text if history is
unavailable or inconsistent.

Describe the change, actual checks, inspected evidence and limitations. Complete
checklists only with supporting evidence. Include useful supplied findings without
inventing scores or requiring review tables. Do not copy other PRs' claims, issue
references or approvals. Preserve unrelated user-authored content. Check text and
attachments for private data; prefer accessible evidence links.

**Publish**

- Create new PRs as drafts, even with passing checks. Preserve existing status,
  including manual changes; never promote, demote or merge.
- Recheck the matching PR and remote head. Invoke commit-push for all commits and
  pushes with prepared task changes, destination, evidence, validation scope and
  the publication blockers above. Use its confirmed result; do not commit or push
  outside that workflow or bypass its blockers.
- Create or update the PR, following host requirements for immediate linking after
  creation or pushes. Verify URL, base, remote head, body and status. Inspect
  remote state before retrying ambiguous API failures to avoid duplicates.
  Use REST if GraphQL is unavailable.

## Result

Return URL, published head, verified draft/ready status, checks and gaps; report
pending CI as pending. Publication ends this workflow; return maintenance to its
caller.

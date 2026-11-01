---
name: file-pr
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# File PR

Run only when the user explicitly invokes `$file-pr` (Codex) or
`/file-pr` (Claude Code), or another skill explicitly instructs you to invoke
this skill.
Mentions, examples and matching tasks alone do not trigger this workflow.

Turn prepared changes into one PR publication with inspected verification.
Invocation authorizes in-scope commits, pushes, PR creation or updates and
publication of inspected evidence.
No prior skill run, council approval or special evidence format is
required. It does not implement features, perform broad cleanup, maintain CI
indefinitely or merge.

## Prepare

1. Identify task files, actual base, branch, remote and matching open PR. Include
   staged, unstaged and untracked changes in inspection; exclude unrelated work
   from commits. Reuse the matching PR and its head branch; otherwise use a task
   branch when on the default branch, detached HEAD or a previously merged branch.
   Infer the target from the request, matching PR and repository conventions;
   ask only if competing targets remain ambiguous. If no matching PR exists and
   the request forbids draft publication, report the conflict before changing
   remote state.
2. Inspect the full proposed diff. Reuse verification that still matches this
   code; otherwise run affected checks and repository-required validation directly.
   No proof or council invocation is necessary. Do not represent a caller's stale
   review as approval of new code.
3. If a required check fails or behavior remains unverified, identify the gap.
   For a new PR or existing draft, disclose the limitation in the draft rather
   than claiming verification passed. For an existing ready PR, return the blocker
   without pushing unverified changes or changing its status. If the request
   requires passing verification before any publication, return the blocker
   before changing remote state.
   Do not repair product code under this skill or ask the user to waive routine
   verification.
4. Establish the project's PR conventions before writing the title and description.
   Read applicable repository instructions, contribution guidance and PR templates,
   including any template selected for this change. Where these leave gaps, inspect
   a few recent merged PRs in the same repository for the usual title format,
   language, description structure and level of detail. Explicit user and repository
   requirements take precedence over conventions inferred from history. If history
   is unavailable or inconsistent, use a concise problem-and-result title and body.
   Follow the resulting format while keeping content specific to this change.
   Include relevant checks, evidence and limitations; complete checklists only
   where supported by actual work. Do not copy another PR's claims, issue references
   or approvals. Add supplied review findings only when useful; do not invent scores
   or require a review table. For updates, preserve unrelated user-authored
   description content. Inspect text and attachments for private data; use accessible
   evidence links where available.

## Publish

Create new PRs as drafts, even after successful verification. When updating an
existing PR, preserve its current status: draft stays draft and ready stays ready,
including a status changed manually by the user. This skill never promotes,
demotes or merges PRs; explicit status transitions belong to a separate workflow.

Recheck the matching PR and remote head before publication. Commit only the
prepared task changes and push to the verified remote and head branch. Never
overwrite remote commits after a rejected push. Reconcile with the remote first;
history rewriting requires authorization and a lease.

Create or update the matching PR, obey the host's immediate PR-linking requirements after
creation or pushes, then verify URL, base, remote head, body and status. Inspect
remote state after ambiguous API failures before retrying to avoid duplicate PRs
or repeated updates. REST is an option when a GraphQL request is unavailable.

## Result

Verify that a new PR is a draft or an existing PR's status was preserved. Return
its URL, published head, actual draft/ready status, verification and gaps.
Publication is this skill's endpoint; pending CI is reported, not called success
of the checks. Hand ongoing maintenance back to a caller that requested it.

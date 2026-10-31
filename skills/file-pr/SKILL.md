---
name: file-pr
description: Commit and publish prepared task changes as a new PR or update the matching PR, with relevant verification and an accurate description. Use independently when implementation is ready to publish.
disable-model-invocation: true
---

# File PR

Turn prepared changes into one verified PR publication. Invocation authorizes
in-scope commits, pushes, PR creation or updates and publication of inspected
evidence. No prior skill run, council approval or special evidence format is
required. It does not implement features, perform broad cleanup, maintain CI
indefinitely or merge.

## Prepare

1. Identify task files, actual base, branch, remote and matching open PR. Include
   staged, unstaged and untracked changes in inspection; exclude unrelated work
   from commits. Use a task branch when on the default branch, detached HEAD or
   a previously merged branch. Infer the target from the request, existing PR
   and repository conventions; ask only if competing targets remain ambiguous.
2. Inspect the full proposed diff. Reuse verification that still matches this
   code; otherwise run affected checks and repository-required validation directly.
   No proof or council invocation is necessary. Do not represent a caller's stale
   review as approval of new code.
3. If a required check fails or behavior remains unverified, identify the gap.
   For a new PR, publish a draft with the limitation clearly stated unless the
   request explicitly excludes drafts. For an existing draft, update it with the
   gap disclosed; for an existing ready PR, return the blocker without pushing
   unverified changes or silently changing its status. If the caller requires
   a verified ready PR, return the blocker instead. Do not repair product code
   under this skill or ask the user to waive routine verification.
4. Write a concise problem-and-result title and description following the repo
   template. Include relevant checks, evidence and limitations. Add supplied
   review findings only when useful; do not invent scores or require a review
   table. Preserve unrelated user-authored description content. Inspect text and
   attachments for private data; use accessible evidence links where available.

## Publish

Commit only the prepared task changes and push to the verified remote. Reuse the
matching PR. Create new PRs as drafts, even after successful verification. Only
the user may mark them ready, or explicitly instruct the agent to do so. A caller
may pass through that explicit instruction but cannot infer it from task completion,
passing checks or a merge request. Preserve an existing PR's status, including
one the user manually marked ready. Never overwrite remote commits after a
rejected push. Reconcile with the remote first; history rewriting requires
authorization and a lease.

Create or update the PR, obey the host's immediate PR-linking requirements after
creation or pushes, then verify URL, base, remote head, body and status. Inspect
remote state after ambiguous API failures before retrying to avoid duplicate PRs
or comments. REST is an option when a GraphQL request is unavailable.

## Result

Return the PR URL, published head, draft/ready state, verification and any gaps.
Publication is this skill's endpoint; pending CI is reported, not called success
of the checks. Hand ongoing maintenance back to a caller that requested it.

---
name: file-pr
description: Trigger this if the user says 'file-pr' or 'file pr', or another skill invokes it.
disable-model-invocation: true
---

# File PR

Publish prepared task changes as a draft PR, or update the matching open PR.
Authorizes task commits, pushes and inspected evidence publication.

1. Inspect the full task diff, including untracked files, branch, remote and actual
   base. Preserve unrelated work; use a fresh task branch if on default, detached
   or already merged. Reuse an existing PR; clarify ambiguous scope or base.
2. Require current [proof](../proof/SKILL.md) and refresh any supplied council
   review invalidated by changes. Standalone use obtains missing proof; callers
   receive preparation blockers. Evidence format belongs to proof. Attach or link
   visual artifacts for reviewers; use adequate alternatives if capture/upload
   fails. Any remaining verification or access gap needs explicit acceptance.
3. Write a concise problem-and-result description following repo conventions.
   Include checks, evidence, accepted gaps and supplied council rounds/scores in
   a `Council review` table. Preserve unrelated user content; omit AI attribution.
   Check text and attachments for private data.
4. Commit only task changes, push to the verified remote and confirm remote head.
   Create a draft or update the existing PR, immediately following harness PR
   linking rules after pushes/creation. Verify URL, base, head, status and evidence
   access. On GraphQL exhaustion, use REST with `draft=true` for creation.

Do not implement, simplify or run council here. Never mark ready, merge,
duplicate an open PR or force-push after rejection. Preserve existing PR status.
Return the PR URL, revision and evidence or a concrete blocker.

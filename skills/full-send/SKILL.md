---
name: full-send
description: Trigger this if the user says 'full-send' or 'full send', or another skill invokes it.
disable-model-invocation: true
---

# Full Send

Deliver a verified change as a draft PR. Authorizes implementation, local runs,
task commits/pushes, rebasing with `--force-with-lease`, evidence publication and
PR maintenance. No checkpoint before draft creation; ready/merge needs a separate
explicit request.

1. Prepare a task branch against the actual base, preserving unrelated work.
   Start implementation and use [jira-status](../jira-status/SKILL.md).
2. Implement, update against the target, resolve conflicts and run
   [simplify](../simplify/SKILL.md).
3. Run [proof](../proof/SKILL.md), then [council](../council/SKILL.md).
   Apply justified in-scope findings and repeat affected proof and council until
   satisfied. Stop on unavailable reviewers, unresolved disagreement or a required
   decision; do not loop without progress. A specific proof gap needs explicit
   acceptance and must remain disclosed.
4. Publish through [file-pr](../file-pr/SKILL.md), update Jira to reflect the
   completed published change, then run [babysit-pr](../babysit-pr/SKILL.md).

Load linked skills when used; they own their details. Preserve scope, revision,
evidence, council rounds and PR across turns; resume the pending stage.

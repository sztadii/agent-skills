---
name: full-review
description: Trigger this if the user says 'full-review' or 'full review', or another skill invokes it.
disable-model-invocation: true
---

# Full Review

Review someone else's PR using local execution and independent council.
Do not edit the author's branch, change Jira/PR status, merge, or submit GitHub
APPROVE/REQUEST_CHANGES. Keep probes isolated and preserve local work.

1. Inspect the exact head, actual base, repo guidance and discussion. Run
   [proof](../proof/SKILL.md) read-only and [council](../council/SKILL.md).
   Continue static review if execution is blocked; disclose limits. Report
   conflicts instead of fixing them. Council assesses simplicity without edits.
2. Verify and rank findings, distinguishing bugs, simplifications and pre-existing
   issues. Report reviewed SHA, evidence and limitations. If head changes,
   refresh affected proof/review before publishing.
3. Show exact inline comments with stable IDs and ask which to publish. Revalidate
   the approved set against current head and publish resolvable threads on valid
   diff lines using [leaving-pr-comments](../leaving-pr-comments/SKILL.md).
   Report unanchorable findings instead of posting top-level comments. New
   findings need approval.
4. Follow agreed threads autonomously: read replies/latest source, verify claimed
   fixes, then reply with evidence. Resolve only own threads after a verified fix
   or explained withdrawal and a successfully posted reply. “Fixed”, outdated
   diffs and green CI alone are insufficient.

Use [watching](../babysit-pr/references/watching.md) while agreed findings remain
open; do not invoke PR maintenance. Preserve pending findings and reviewed SHA
across wakes. Report completion only when agreed findings are settled; stop on
user request, closure or a required decision.

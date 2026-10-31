---
name: full-review
description: Review a PR or local change thoroughly, verify actionable findings and follow up on published feedback. Use for an independent assessment without editing the author's implementation or starting feature delivery.
disable-model-invocation: true
---

# Full Review

Own a review from inspection through substantiated feedback. Accept a PR, commit
range or local diff; infer an unambiguous target from context. No prior proof,
council or full-send run is required. Keep the author's code and unrelated local
work unchanged; use isolated probes for execution.

For a PR, invocation includes publishing verified review comments and following
up on this review's threads. A report-only/draft request or a local-only target
ends with local findings. Do not require a separate approval checkpoint for
publication already included in scope. Do not change PR status, push fixes,
merge or submit APPROVE/REQUEST_CHANGES unless explicitly requested.

## Review

1. Read intent, repository guidance, actual base, exact head and complete diff,
   including relevant local/untracked changes. Read existing PR discussion to
   avoid duplicate findings. Check correctness, regressions, integration,
   maintainability and tests; distinguish introduced defects from existing issues.
2. Run repository-required and affected checks when feasible, and reproduce
   suspected failures with isolated probes. If execution is blocked, continue
   static review and label the limitation rather than pretending verification.
3. Use two independent reviewers with inherited settings by default, following
   the host's delegation rules. Give them the stable subject, intent and evidence;
   reviewers must not edit, publish or delegate. Honor user-selected reviewers
   without silent substitutions. If delegation is unavailable, finish the direct
   review and disclose the missing independent coverage. A council skill, when
   installed, may supply these reviews but is not a prerequisite.
4. Validate and deduplicate findings. Each actionable item needs severity, precise
   location, a concrete trigger and consequence, and supporting evidence. Separate
   blocking defects from optional simplifications. Investigate disagreement; do
   not post a speculative finding just because a reviewer suggested it.

## Communicate and follow up

Return findings locally when publication is outside scope. Otherwise re-read the
PR head and refresh affected findings before posting. Use valid inline diff
anchors for line-specific findings and a concise review summary for general
issues or verification limits. Submit COMMENT by default. Attribute agent-authored
messages with the verified model/host and user login on a separate line:
`🤖 **<model> responding on behalf of <user>**`. Inspect remote state before retrying
an uncertain post and verify each published URL. The leaving-pr-comments skill
can handle formatting/publication if available, but is not required.

For published findings, read replies and updated code, verify fixes and reply
with evidence. Resolve only this workflow's own threads after a verified fix or
explained withdrawal and successful reply. Neither a “fixed” reply nor green CI
proves the finding is settled. Track the reviewed revision across changes.

If agreed follow-up remains, use the host's PR event watcher, save pending thread
IDs, reviewed head and next actions, then yield as the host requires. In T3 Code
only the parent watches; unwatch on completion, cancellation or blocked handback.
Do not poll or start babysit-pr: the author owns implementation. Without a working
watcher, report pending findings and the monitoring limitation honestly.

Finish with the reviewed revision, findings, evidence, limitations, published
URLs and thread state. No findings is a valid result; unresolved published
findings remain pending rather than being reported as a completed follow-up.

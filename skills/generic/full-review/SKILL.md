---
name: full-review
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Full Review

Run only when the user explicitly invokes `$full-review` (Codex) or
`/full-review` (Claude Code), or another skill explicitly instructs you to invoke
this skill.
Mentions, examples and matching tasks alone do not trigger this workflow.

## Required skill check — first action

Required skills: [$leaving-pr-comments](../leaving-pr-comments/SKILL.md).

At entry, check once through the host's skill discovery that the skills listed
above are installed and their `SKILL.md` files are readable. If any are unavailable,
stop the entire workflow, including the caller, and tell the user which are
missing. Do not skip, substitute or auto-install them. After this check succeeds,
proceed with the workflow below without repeating the availability check.

## Purpose and scope

Own a review from inspection through substantiated feedback. Accept a PR, commit
range or local diff; infer an unambiguous target from context. No prior proof,
council or full-send run is required. Keep the author's code and unrelated local
work unchanged; use isolated probes for execution.

For a PR, first present verified findings and proposed comments to the user in
the conversation. Wait for the user's explicit approval of the presented comments
before publishing any of them, even when the initial request includes publication.
Approval may cover all comments or a selected subset. A report-only/draft request
or a local-only target ends with local findings. Do not change PR status, push
fixes, merge or submit APPROVE/REQUEST_CHANGES unless explicitly requested.

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
   review and disclose the missing independent coverage. Use independent reviewers
   directly, not council's code-preparation workflow: this review must assess the
   author's actual code without simplification edits.
4. Validate and deduplicate findings. Each actionable item needs severity, precise
   location, a concrete trigger and consequence, and supporting evidence. Separate
   blocking defects from optional simplifications. Investigate disagreement; do
   not post a speculative finding just because a reviewer suggested it.

## Communicate and follow up

Present the reviewed revision, findings, evidence, limitations and proposed PR
comment text before asking which comments to publish. If there are no actionable
findings, return that result without requesting publication approval. For a PR
review that includes publication, pause for human approval here; do not publish
or start a watcher while awaiting it. Report-only and local reviews end here.

After approval, re-read the PR head and refresh affected findings before posting.
Publish only approved comments; omit findings that are already fixed. New findings
or material changes to approved claims require presenting the revised text for
approval again. Pass only approved comments and their publication scope to any
helper; invoking leaving-pr-comments does not bypass this checkpoint.
Use valid inline diff anchors for line-specific findings and a concise review
summary for general issues or verification limits. Submit COMMENT by default. Attribute agent-authored
messages with the verified model/host and user login on a separate line:
`🤖 **<model> responding on behalf of <user>**`. Inspect remote state before retrying
an uncertain post and verify each published URL. Invoke
[$leaving-pr-comments](../leaving-pr-comments/SKILL.md) for approved publication
and replies; the user's approval checkpoint above still applies.

For approved published findings, read replies and updated code, verify fixes and
reply with evidence within those findings' scope. New findings still require
human approval before publication. Resolve only this workflow's own threads after
a verified fix or explained withdrawal and successful reply. Neither a “fixed”
reply nor green CI proves the finding is settled. Track the reviewed revision
across changes.

If agreed follow-up remains, use the host's PR event watcher, save pending thread
IDs, reviewed head and next actions, then yield as the host requires. In T3 Code
only the parent watches; unwatch on completion, cancellation or blocked handback.
Do not poll or start babysit-pr: the author owns implementation. Without a working
watcher, report pending findings and the monitoring limitation honestly.

Finish with the reviewed revision, findings, evidence, limitations, published
URLs and thread state. No findings is a valid result; unresolved published
findings remain pending rather than being reported as a completed follow-up.

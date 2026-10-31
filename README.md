# Agent skills

Eight focused skills for autonomous development. Each accepts the initial task
context, does its own work and returns a concrete result. None requires a prior
full-send run. Full-send composes the delivery process when that is what you ask for.

## Install and invoke

```sh
npx skills add sztadii/agent-skills -g --skill '*'
```

Install just one with `npx skills add sztadii/agent-skills -g --skill simplify`.
Use `$skill-name`, its name in a request, or invoke it from another workflow.
Explicit-invocation settings are preserved in `SKILL.md` for hosts that support
that flag and in `agents/openai.yaml` for Codex. Load a skill's instructions when using it.
The repository is the source; installed copies need an update to receive changes.

## Choose the result

| Skill | Owns | Endpoint when used alone |
| --- | --- | --- |
| `simplify` | Focused cleanup and affected validation | Clearer, verified local diff, or justified no-op |
| `proof` | Behavioral verification and inspected evidence | Passed, failed or incomplete claims; no product edits |
| `council` | Independent agent reviews and reconciliation | Findings and clear / changes needed / incomplete verdict |
| `file-pr` | Task commits, push and PR creation/update | Published PR with current evidence and status |
| `leaving-pr-comments` | Requested comments, replies and thread resolution | Verified posted URLs, or text for a draft-only request |
| `babysit-pr` | Existing PR's CI, feedback and conflicts | Settled required checks/reviews, or requested merge |
| `full-review` | Thorough review and follow-up on its findings | Local report, or published findings with settled threads |
| `full-send` | Implementation through delivery and maintenance | Verified draft PR, or explicitly requested merge/deployment |

Focused skills include the checks needed for their own result. Simplify does not
need proof, publishing does not need council, and babysit-pr does not need file-pr.
Full-review performs its own review and may reuse council or leaving-pr-comments.
Full-send can use the focused skills when installed; its instructions also cover
the phases when installed alone. There is no mandatory circular workflow.

## Deliver from initial context

> Full-send: prevent duplicate form submissions. Preserve the existing API and
> cover retry after a failed submission.

The agent establishes acceptance criteria, implements and simplifies the change,
verifies behavior, obtains independent review, addresses valid findings, publishes
a draft PR and handles CI, feedback and conflicts. It makes routine choices and
continues between phases without asking you to approve each transition.

The default endpoint is a verified draft PR with available current-head checks
and reviews settled and relevant feedback addressed. New PRs are created as drafts.
Only you may mark a PR ready, or explicitly instruct the agent to do so. Full-send,
passing checks, completed reviews and a merge request alone do not authorize that
transition. Existing PRs retain their status, including when you manually mark
them ready. Requirements that only run after promotion are disclosed as deferred,
never reported as passed, and do not trigger automatic promotion.

Include an explicit ready instruction along with “merge when checks and reviews
pass”, or specify a deployment destination, when that is the requested endpoint.
Required human approvals and missing access cannot be fabricated or bypassed.

If essential verification or independent review cannot complete, the agent keeps
useful work in a clearly labelled draft and reports incomplete delivery with the
specific gap. It does not ask you to routinely waive checks or report an unfinished
PR as delivered. Optional artifacts or an absent optional bot do not block work.
No skill updates an issue tracker as a side effect.

## Use each skill independently

> Simplify this diff while preserving behavior and useful regression coverage.

> Proof: reproduce this CLI bug and verify whether the current change fixes it.

> Council: review this migration plan for compatibility and rollback risks.

> File-pr: publish the prepared pagination fix as a draft with its evidence.

> Leaving-pr-comments: post a reply on this thread explaining the verified fix.

> Babysit-pr: address valid feedback and CI failures on PR #84; keep it draft.

> Full-review PR #91: verify the export flow and follow up on actionable findings.

> Full-review this local diff; return a report only.

A PR full-review includes publishing substantiated comments and following its own
threads, without an extra comment-selection checkpoint. A report-only or local
review does not publish. Requests to draft or reword a comment return text only.
Reviews leave the author's implementation unchanged and default to COMMENT rather
than formal APPROVE or REQUEST_CHANGES decisions.

Council uses two independent agents on the current provider with inherited
settings unless you specify reviewers. Explicit provider/model choices are never
silently replaced. Missing reviewers are disclosed; optional stylistic objections
do not create an endless approval loop.

## Continue across events

Retain scope, endpoint, revision, evidence and pending work across turns. Refresh
verification and review only where changes invalidate them. In T3 Code the owning
parent handles immediate work, registers the PR event watcher and yields;
delegated reviewers never watch. Resume the pending phase on events. Stop watches
on completion, cancellation or blocked handback. There is no custom polling script.
Without a working watcher, disclose that maintenance is not continuing in the
background. Distinguish waiting on an external event from completed delivery.

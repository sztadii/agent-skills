---
name: full-review
description: Trigger this if the user says 'full review'.
---

# Full Review

Review someone else's PR locally, including real execution and unnecessary
complexity. Return findings to the user. Do not modify the author's branch,
commit, push, publish the initial comments or review, change PR status, or
merge unless the user separately asks for that action. Once publishing findings
is authorized, follow up on those findings as described below.

## Prepare

Identify the PR, read its description and existing review discussion, and
record its target branch and head SHA. Fetch the target and PR head. Use an
isolated local checkout at that exact head; preserve existing working changes.
Review the diff from the merge base with the actual target branch, not an
assumed main branch. Read repository instructions and nearby code for context.

## Verify locally

Use [proof](../proof/SKILL.md). Set up dependencies and run the application or
relevant executable path, exercising the changed behavior yourself. For UI,
interact with the actual UI; for services or CLI changes, exercise the affected
endpoint or command. Cover the intended flow and relevant failure cases.
Do not substitute reading code, mocked tests, or green CI for local execution.
For changes with no runnable behavior, such as prose-only edits, explain that
and use the appropriate direct check instead.

The request authorizes ordinary local setup, app execution, and verification.
Use local test data and test services; do not run against production or perform
destructive setup without separate authorization. Keep temporary probes and
artifacts separate from the author's change. Stop only processes you started.
If setup or execution is blocked, continue the useful static review and report
the limitation; do not present the PR as fully verified.

## Review the change

Check correctness, regressions, and integration with surrounding code. Inspect
checks and merge conflicts against the target branch; report conflicts rather
than rebasing or fixing someone else's PR.

Look for unnecessary abstractions, branches, speculative handling, duplicate
test setup, and comments that repeat code. Prefer existing repository patterns
and local solutions. Give concrete smaller alternatives while preserving
behavior, meaningful assertions, validation, and error handling. Do not reward
line-count reductions that hide complexity or suggest unrelated refactors.

Verify each finding against the code or observed behavior, and avoid repeating
already resolved feedback. Distinguish pre-existing failures from regressions
introduced by the PR, using the base revision when needed.

## Initial report and comment approval

Recheck the PR head before finishing. If it changed, review the new diff and
rerun affected verification before giving a current verdict; otherwise clearly
limit the report to the SHA actually reviewed.

Report the PR link and reviewed SHA, findings ordered by impact with file/line
references and concrete evidence, and what you ran and observed locally.
Separate bugs from optional simplifications. Include short recordings or other
local evidence when useful, plus any unverified areas. If there are no findings,
say so without implying that untested behavior was verified.

Before the first publication, show the user the exact proposed comments with
stable IDs and their target file/line or thread. Format them with
[leaving-pr-comments](../leaving-pr-comments/SKILL.md). Wait for the user to
select or approve them; publish only that agreed set as inline review comments
on the relevant diff lines, creating resolvable review threads. Do not publish
findings as top-level PR conversation comments or a summary-only review. Reply
in the same inline thread during follow-up. If a finding cannot be anchored to
a valid diff line, report that limitation to the user rather than silently
posting it to the main conversation. Keep rejected findings
out of the published review and follow-up, and do not reintroduce them without
new evidence. Recheck that approved comments still apply if the PR head changed.
Never submit GitHub APPROVE in this workflow; the user does that manually.
Approval to post comments does not authorize REQUEST_CHANGES either.

## Follow up on published findings

After publishing findings, track their review-thread IDs and monitor replies
and new commits until those findings are settled. Reply autonomously using
[leaving-pr-comments](../leaving-pr-comments/SKILL.md); keep replies to one
sentence where possible, two at most. Do not ask for approval of each reply,
verification, or resolution within the agreed findings. Do not repeat unchanged
feedback. Bring genuinely new findings outside that agreed set to the user
before publishing them.

On a claimed fix or new head, fetch the current PR head, inspect the relevant
change, and rerun the affected local verification with `proof`. Check outstanding
findings even if their comments predate the latest push. Resolve only your own
review threads, after verifying the fix and successfully posting a brief
confirmation. A claim of "fixed", an outdated diff, or green CI alone is not
enough. If your finding was mistaken, explain its withdrawal before resolving.
Leave other reviewers' threads alone and report conflicts instead of fixing them.

In T3 Code, the parent thread running this review uses `watch_pull_request`
and ends its turn; delegated reviewers never watch. Handle existing replies
and updates before rearming the watch. Use one watcher, never a polling or
sleep loop. A green check does not end follow-up while findings remain open.
If watching is unavailable, report the limitation rather than promising follow-up.

Stop when your findings are settled, the PR closes or merges, the user stops
review, or further progress needs the user's input. Call `unwatch_pull_request`
before handing back or asking a question, and report remaining open findings.
Do not modify the author's branch or merge during follow-up.

When the agreed findings are settled, recheck the current head and relevant
checks, then tell the user "Review done" with the reviewed SHA, verification
outcome, and any remaining limitations. Distinguish completion of the agreed
review from a claim that every aspect of the PR is correct. Leave the final
GitHub APPROVE action to the user to perform manually; never submit it yourself.
If verification is blocked or findings remain open, report that state instead
of claiming the review is successfully complete.

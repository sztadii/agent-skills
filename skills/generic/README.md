# Generic skills

[Back to main README](../../README.md)

Reusable skills for implementation, verification, review and PR delivery across
projects and companies. Use `$full-send` for the whole delivery process or pick
a skill for a specific task.

Invoke skills explicitly with `$skill-name` in Codex or `/skill-name` in Claude
Code. Another skill can also explicitly invoke them. `commit-push` is the
exception: always invoke it when creating Git commits or pushing commits,
without requiring an explicit skill request.

## Skills

- [grill-me](grill-me/SKILL.md) — Stress-tests a plan or design through rounds of questions and recommendations before implementation; adapted from Matt Pocock's workflow.
- [find-improvements](find-improvements/SKILL.md) — Analyzes current repository code and proposes two small bugs and two refactorings without making changes.
- [full-send](full-send/SKILL.md) — Delivers a change from implementation through verification, independent review, PR publication and maintenance.
- [full-pr-review](full-pr-review/SKILL.md) — Reviews a PR through `$proof` and `$council`, asks before publishing comments, and monitors changes until user approval, merge or closure.
- [simplify](simplify/SKILL.md) — Reduces task code complexity while preserving behavior and useful test coverage.
- [proof](proof/SKILL.md) — Verifies behavior and reports what passed, failed or remains unverified.
- [council](council/SKILL.md) — Coordinates independent reviews of code or a proposal without changing the reviewed version.
- [commit-push](commit-push/SKILL.md) — Commits prepared changes, pushes existing commits, or both, with hooks and final-output verification.
- [file-pr](file-pr/SKILL.md) — Checks prepared changes, commits and pushes them, and creates a draft PR or updates an existing PR.
- [babysit-pr](babysit-pr/SKILL.md) — Maintains a PR by addressing substantiated feedback, CI failures and integration conflicts.
- [leaving-pr-comments](leaving-pr-comments/SKILL.md) — Drafts or publishes concise, verified PR comments and replies.

## Examples: single skill

> $grill-me Stress-test my plan to migrate authentication before we implement it.

Examines the proposal in rounds, verifies repository facts and resolves dependent
decisions before summarizing the agreed direction for confirmation.

> $find-improvements

Analyzes the current repository for ten minutes and proposes two small bugs and
two refactorings without making changes.

> $full-send Prevent duplicate form submissions and deliver a verified draft PR.

> $full-send Implement https://example.atlassian.net/browse/APP-123 and deliver a verified draft PR.

> $full-send Implement APP-456. The timeout happens only when uploading files larger than 10 MB; preserve the existing retry behavior and add regression coverage.

`$full-send` already coordinates several skills: `$simplify`, `$proof`,
`$council`, `$file-pr` (which uses `$commit-push`) and `$babysit-pr` (which calls
`$leaving-pr-comments` for PR communication). One invocation covers the delivery
workflow; you do not need to list those skills separately.

> $full-pr-review Review PR #91 and show me proposed comments before publishing.

> $simplify Streamline my current task diff while preserving behavior and useful test coverage.

> $proof Verify that the local form changes prevent duplicate submissions, including after a failed request.

> $council Evaluate this API migration proposal with two independent reviewers and report their decision and objections.

> $file-pr Verify the prepared changes and publish them as a draft PR.

> $commit-push Commit the prepared changes without pushing.

> $babysit-pr Maintain PR #91: address substantiated feedback and CI failures, and push verified fixes.

> $leaving-pr-comments Draft a concise comment for PR #91 explaining how a failed request leaves the submit button disabled.

## Examples: combining skills

> $simplify then $proof: streamline my current task diff, then verify that the changed behavior meets the acceptance criteria.

> $proof then $file-pr: verify my prepared fix and, if all required checks pass, publish it as a draft PR with the evidence.

> $simplify then $council then $file-pr: streamline and independently review my prepared changes, then publish a draft PR only if there are no blocking findings and required verification passes.

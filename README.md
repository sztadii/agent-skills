# Agent skills

Skills for coding agents to implement changes, simplify code, verify behavior
and review PRs. Use `$full-send` for the whole delivery process or pick a skill
for a specific task.

All skills require explicit invocation: `$skill-name` in Codex, `/skill-name` in
Claude Code, or an explicit call from another skill. Mentions, examples and
matching tasks alone do not trigger them.

Codex disables implicit invocation through `agents/openai.yaml`. Claude Code
uses the descriptions and instructions to limit invocation while allowing skills
to call each other.

Install all skills:

```sh
npx skills add sztadii/agent-skills -g --skill '*'
```

## Skills

- [full-send](skills/full-send/SKILL.md) — Delivers a change from implementation through verification, independent review, PR publication and maintenance.
- [full-review](skills/full-review/SKILL.md) — Reviews a PR or diff and presents verified findings; PR comments require approval before publication.
- [simplify](skills/simplify/SKILL.md) — Simplifies the task's code while preserving behavior and useful test coverage.
- [proof](skills/proof/SKILL.md) — Verifies behavior and reports what passed, failed or remains unverified.
- [council](skills/council/SKILL.md) — Coordinates independent reviews of code or a proposal, simplifying code before review.
- [file-pr](skills/file-pr/SKILL.md) — Checks prepared changes, commits and pushes them, and creates a draft PR or updates an existing PR.
- [babysit-pr](skills/babysit-pr/SKILL.md) — Maintains a PR by addressing substantiated feedback, CI failures and integration conflicts.
- [leaving-pr-comments](skills/leaving-pr-comments/SKILL.md) — Drafts or publishes concise, verified PR comments and replies.

## Examples ( single skill )

> $full-send Prevent duplicate form submissions and deliver a verified draft PR.

> $full-send Implement https://example.atlassian.net/browse/APP-123 and deliver a verified draft PR.

> $full-send Implement APP-456. The timeout happens only when uploading files larger than 10 MB; preserve the existing retry behavior and add regression coverage.

`$full-send` already coordinates several skills: `$proof`, `$council` (which
calls `$simplify`), `$file-pr` and `$babysit-pr` (which calls
`$leaving-pr-comments` for PR communication). One invocation covers the delivery
workflow; you do not need to list those skills separately.

> $full-review Review PR #91 and show me proposed comments before publishing.

> $simplify Simplify my current task diff while preserving behavior and useful test coverage.

> $proof Verify that the local form changes prevent duplicate submissions, including after a failed request.

> $council Evaluate this API migration proposal with two independent reviewers and report their decision and objections.

> $file-pr Verify the prepared changes and publish them as a draft PR.

> $babysit-pr Maintain PR #91: address substantiated feedback and CI failures, and push verified fixes.

> $leaving-pr-comments Draft a concise comment for PR #91 explaining how a failed request leaves the submit button disabled.

## Examples ( multi )

> $simplify then $proof: simplify my current task diff, then verify that the changed behavior meets the acceptance criteria.

> $proof then $file-pr: verify my prepared fix and, if all required checks pass, publish it as a draft PR with the evidence.

> $council then $file-pr: simplify and independently review my prepared changes, then publish a draft PR only if there are no blocking findings and required verification passes.

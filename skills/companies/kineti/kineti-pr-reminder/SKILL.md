---
name: kineti-pr-reminder
description: "Trigger only when the user or another skill explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti PR review reminders

Personal workflow for GitHub author `sztadii` in organization `kineti1`.
Run only when explicitly invoked as `$kineti-pr-reminder` (Codex),
`/kineti-pr-reminder` (Claude Code), or by another skill, including scheduled
invocations. Invocation authorizes sending Slack DMs to everyone in the team
table by default. Use a different recipient scope only when the user explicitly
requests it; no recipient list is needed when invoking the skill.

## Workflow

1. Fetch all open, non-draft PRs by `sztadii` across `kineti1`, including new
   repositories and all pages. Keep only PRs with no pending or failed checks
   or commit statuses on the current head, no unresolved review threads and no
   merge conflicts. Skipped/neutral checks or no configured checks are fine;
   `Changes requested` alone is not a blocker. Skip and report unverifiable PRs.
2. For every teammate in the recipient scope who works today in
   `Europe/Warsaw`, select PRs without
   **their own valid approval**. Use GitHub's approval state: a push does not
   invalidate an approval unless GitHub does so. Other people's approvals and
   the requested-reviewer list do not restrict this selection.
3. Recheck eligibility before sending. Fetch each PR's `title`, `url`,
   `additions` and `deletions` (e.g. `gh pr view <URL> --json
   title,url,additions,deletions`). Send one English DM per nonempty recipient
   list, using the format below. Use the listed DM IDs directly; do not guess
   identities or invent missing data. Resolve any user-requested recipient
   outside the table before sending. Do not retry an uncertain
   send.
4. Report message links and PR counts, or reasons for skipping. Do not change
   PRs or reviews. Eligible PRs may repeat in later runs even without changes.
   Do not catch up missed runs or create schedules; the two existing daily runs
   are intentional.

## Team

Default recipients: everyone below, subject to their listed working days in
`Europe/Warsaw` and per-person approval checks. Only an explicit user instruction
changes the recipient scope.

| Name | GitHub ID (login) | Slack user ID | Email | Slack DM | Working days |
| --- | --- | --- | --- | --- | --- |
| Oleh Botsiun | `Boto1` | `U082BATBUF4` | oleh@kineti.nl | `D08FRL221RB` | Monday–Friday |
| Michał Grzeżułkowski | `michalg-kineti` | `U08CFEQBYSX` | michal.g@kineti.nl | `D08FRUSPBHB` | Monday–Friday |
| Jakub | `jakubwkineti` | `U09GBBXTHHT` | jakub.w@kineti.nl | `D08G9H6N15H` | Tuesday–Thursday |

## Example message

Illustrative titles, counts and links; replace them with current PR data.

> These PRs are ready for review:
> - Fix duplicate form submissions — 100 added, 50 removed — https://github.com/kineti1/REPO/pull/123
> - Improve validation messages — 25 added, 10 removed — https://github.com/kineti1/REPO/pull/124

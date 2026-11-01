---
name: kineti-pr-reminder
description: "Trigger only when the user or another skill explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti PR review reminders

Run only when explicitly invoked with `$kineti-pr-reminder` (Codex),
`/kineti-pr-reminder` (Claude Code), or by another skill. Scheduled runs must
explicitly name this skill. Editing it or mentioning it in an example does not
invoke it. Invocation authorizes sending the reminder to the established Slack
destination.

## Workflow

1. Identify the authenticated GitHub user (`gh api user --jq .login`). Fetch all
   their open, non-draft PRs across the `kineti1` organization, including new
   repositories and all result pages.
2. Keep only fully ready PRs: current-head checks and commit statuses have no
   pending or failed results, all review threads are resolved, and there are no
   merge conflicts. Skipped or neutral checks and no configured checks are fine;
   failed data retrieval is not evidence of no checks. `Changes requested` alone
   is not an extra blocker. Skip PRs whose readiness cannot be verified and
   report why.
3. Build a separate list for each teammate below, excluding PRs where that
   teammate already has a valid approval. Follow GitHub's current approval state
   and repository rules: an approval remains valid after a push unless GitHub
   invalidates it. Another person's approval does not remove the PR from this
   teammate's list. Requested reviewers do not restrict recipients.
4. Check today's weekday in `Europe/Warsaw` and skip teammates outside their
   working days. Recheck PR readiness and approvals immediately before sending.
   Fetch `title`, `url`, `additions` and `deletions` for each selected PR
   (for example, `gh pr view <URL> --json title,url,additions,deletions`). Counts
   describe added and removed lines in the current PR diff; do not invent them.
5. Send at most one English message per teammate to their listed Slack DM, only
   if their list is nonempty. Each entry contains only the PR title, line counts
   (`100 added, 50 removed`) and link. Do not add mentions or broadcast pings.
   Report missing data or access as a blocker. Do not retry an uncertain send.
6. Report sent messages with links and PR counts, or reasons for skipping.
   Do not change PRs or reviews. A still-eligible PR may appear again in the next
   scheduled run even without new changes. Do not catch up missed runs or create
   extra sends or schedules; the two existing daily runs are intentional.

## Team

These are the only recipients. GitHub-to-DM mappings and working days come from
user-provided configuration in the existing PR reminder schedulers, checked on
2026-11-01. Slack user IDs and emails come from Kineti Slack. Use the listed DM
conversation IDs directly without rediscovering identities. Working days use
`Europe/Warsaw`.

| Name | GitHub ID (login) | Slack user ID | Email | Slack DM | Working days |
| --- | --- | --- | --- | --- | --- |
| Oleh Botsiun | `Boto1` | `U082BATBUF4` | oleh@kineti.nl | `D08FRL221RB` | Monday–Friday |
| Michał Grzeżułkowski | `michalg-kineti` | `U08CFEQBYSX` | michal.g@kineti.nl | `D08FRUSPBHB` | Monday–Friday |
| Jakub | `jakubwkineti` | `U09GBBXTHHT` | jakub.w@kineti.nl | `D08G9H6N15H` | Tuesday–Thursday |

## Example message

Illustrative titles, counts and links; replace them with current PR data.

> Hi! These PRs are ready for review:
> - Fix duplicate form submissions — 100 added, 50 removed — https://github.com/ORG/REPO/pull/123
> - Improve validation messages — 25 added, 10 removed — https://github.com/ORG/REPO/pull/124
>
> Thanks for taking a look!

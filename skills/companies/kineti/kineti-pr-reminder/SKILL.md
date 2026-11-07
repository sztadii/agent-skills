---
name: kineti-pr-reminder
description: "Trigger only when the user explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti PR review reminders

## Scope

Find reviewable PRs by `sztadii` across `kineti1`; send one English Slack DM per
eligible teammate. Invocation, including scheduled runs, authorizes private
one-to-one DMs to the team below; change recipients only on explicit request.
Do not post in channels, modify PRs/reviews, catch up missed runs or change schedules. The two
daily runs are intentional; PRs may repeat later.

## Workflow

**Find PRs**

Use connected tools, or authenticated gh when required operations are missing.
Check `gh auth status` and try read-only CLI/API calls before reporting unavailable
access. Search all repositories, not just the launch project:

```sh
gh api --method GET --paginate search/issues -f q='org:kineti1 author:sztadii is:pr is:open draft:false'
```

Retrieve all pages; split by creation date if GitHub's 1,000-result cap applies.
Read URL, author login, state, draft flag, headRefOid, mergeability, checks/statuses,
reviews, additions and deletions. Paginate reviews, unresolved threads and comments
using GraphQL as needed; identify each unresolved thread's latest comment and author.
Omitted/truncated data is not empty. Select only needed fields (`--jq`/`--json`);
if output is cut off, rerun narrower queries rather than skipping the PR. Inspect
current-head check runs and commit statuses; a combined status of `pending` with
no statuses means none are configured. Retry UNKNOWN mergeability with a fresh read.

Keep open, non-draft PRs without pending/failed checks or conflicts. An unresolved
thread blocks reminders only if its latest comment is not by the PR author.
Otherwise, the author awaits a reply: treat the thread as handled for reminder
eligibility without resolving it on GitHub. PRs with only such unresolved threads
remain eligible for teammates without their own valid approval.

Neutral/skipped/no configured checks are acceptable; Changes requested alone is
not a blocker. Skip unverifiable PRs and report actual API/auth failures.

**Send**

For each scoped teammate working today in Europe/Warsaw, select eligible PRs
without their own valid approval. Use GitHub review state, including dismissals;
pushes invalidate approvals only if GitHub does. Other approvals and requested
reviewers do not restrict selection.

Recheck eligibility/head before sending; reevaluate changed heads. Fetch current
URL/additions/deletions; send one DM per nonempty list in the format below, without
titles or invented data. Send only to the listed Slack DM channel ID and confirm
the returned channel matches it; report any mismatch. Several Kineti people share
first names (for example three Jakubs), so never look up a teammate's Slack
recipient by name, email or GitHub login. Resolve outside recipients before
sending. Do not retry uncertain sends.

**Team**

| Name                 | GitHub ID (login) | Email              | Slack DM channel ID | Working days     |
| -------------------- | ----------------- | ------------------ | ------------------- | ---------------- |
| Oleh Botsiun         | `Boto1`           | oleh@kineti.nl     | `D08FRL221RB`       | Monday–Friday    |
| Michał Grzeżułkowski | `michalg-kineti`  | michal.g@kineti.nl | `D08FRUSPBHB`       | Monday–Friday    |
| Jakub                | `jakubwkineti`    | jakub@kineti.nl    | `D08G9H6N15H`       | Tuesday–Thursday |

**Message**

Send only the PR list below, with no introduction, replacing the illustrative
URLs/counts with current data:

> - https://github.com/kineti1/REPO/pull/123 — 100 added, 50 removed
> - https://github.com/kineti1/REPO/pull/124 — 25 added, 10 removed

## Result

Report message links and PR counts or skip reasons.

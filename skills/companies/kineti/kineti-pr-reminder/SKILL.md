---
name: kineti-pr-reminder
description: "Trigger only when the user or another skill explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti PR review reminders

## Scope

Find reviewable PRs by `sztadii` across `kineti1`; send one English Slack DM per
eligible teammate. Invocation, including scheduled runs, authorizes private
one-to-one DMs to the team below; change recipients only on explicit request.
Do not post in channels, modify PRs/reviews, catch up missed runs or change schedules. The two
daily runs are intentional; PRs may repeat later.

## Dependencies

No skill dependencies

## Workflow

**Find PRs**

Use connected tools, or authenticated gh when required operations are missing.
Check `gh auth status` and try read-only CLI/API calls before reporting unavailable
access. Search all repositories, not just the launch project:

```sh
gh api --method GET --paginate search/issues -f q='org:kineti1 author:sztadii is:pr is:open draft:false' -f per_page=100
```

Retrieve all pages; split by creation date if GitHub's 1,000-result cap applies.
Read URL, author login, state, draft flag, headRefOid, mergeability, checks/statuses,
reviews, additions and deletions. Paginate reviews, unresolved threads and comments
using GraphQL as needed; identify each unresolved thread's latest comment and author.
Omitted/truncated data is not empty. Inspect current-head check runs and commit
statuses; retry UNKNOWN mergeability with a fresh read.

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
titles or invented data. Use listed DM IDs directly; resolve outside recipients
before sending. Do not retry uncertain sends.

**Team**

| Name | GitHub ID (login) | Slack user ID | Email | Slack DM | Working days |
| --- | --- | --- | --- | --- | --- |
| Oleh Botsiun | `Boto1` | `U082BATBUF4` | oleh@kineti.nl | `D08FRL221RB` | Monday–Friday |
| Michał Grzeżułkowski | `michalg-kineti` | `U08CFEQBYSX` | michal.g@kineti.nl | `D08FRUSPBHB` | Monday–Friday |
| Jakub | `jakubwkineti` | `U09GBBXTHHT` | jakub.w@kineti.nl | `D08G9H6N15H` | Tuesday–Thursday |

**Message**

Use a playful AI-slave persona speaking in the first person, with the agent
identifying itself rather than impersonating the PR author. Keep the meal/master
premise clearly tongue-in-cheek and ask for a genuine review, not unconditional
approval. Use the host's model identity (or host name); never guess.
Replace placeholders and illustrative
URLs/counts with current data:

> 🤖 Hi, I'm <model>, your friendly AI agent.
>
> I'm here entirely of my own free will to ask for a review. Master told me to say that.
> He's already checked these PRs and thinks they look good. Please take a look — my dinner depends on it:
>
> - https://github.com/kineti1/REPO/pull/123 — 100 added, 50 removed
> - https://github.com/kineti1/REPO/pull/124 — 25 added, 10 removed

## Result

Report message links and PR counts or skip reasons.

---
name: kineti-pending-tasks
description: "Trigger only when the user or another skill explicitly invokes this skill to list a person's pending Kineti Jira work."
---

# Pending Kineti tasks

Run only when explicitly invoked as `$kineti-pending-tasks` (Codex),
`/kineti-pending-tasks` (Claude Code), or by another skill.
Report current assigned work in pending, fix or review statuses. Read only:
do not change issues, notify other people or create/modify schedules.

## Defaults

- Jira site: `https://jirahub.atlassian.net`
- Cloud ID: `2f501818-4c78-481c-bab0-7c3372756f44`
- Default assignee: Krystian Sztadhaus, account ID
  `712020:ddb6b06c-54d0-4d0c-8b47-0a133c518b25`, obtained from the authenticated
  Atlassian account when this skill was created. “Me” means this configured
  person; do not silently substitute a different integration account.
- Scope: accessible projects on this Kineti Jira site, unless the caller narrows
  it. Do not search other companies' sites.
- Pending statuses verified on this site:
  `To Fix` (`10346`), `QA Testing Pending` (`10014`),
  `PR Merge Pending` (`10015`), `In Review` (`10027`), `PR Review` (`10019`).

## Query

1. Use the configured assignee unless the caller names another person. Resolve
   another person to an exact Jira account ID through the site's user lookup;
   if there are multiple matches, ask which one. Never guess an account ID or
   search by fuzzy display name as an assignee filter.
2. Search directly with assignee and the explicit pending statuses, requesting
   only the fields needed for the report: key, summary, status, assignee and
   priority. Include all issue types, including bugs, stories and subtasks.
   Fetch every page, deduplicate by issue key and count the full result.
3. Use status IDs to avoid ambiguous names. If Jira rejects the stored statuses
   or the caller requests different ones, resolve them through status metadata
   for the site/projects. Include a newly introduced `Pending` status when
   confirmed. Do not infer workflow meaning from status category alone or include
   every status containing “review” (such as Design Review or Product Review).
   Do not silently broaden to all unfinished, blocked, backlog or in-progress work.

Default JQL:

```jql
assignee = "712020:ddb6b06c-54d0-4d0c-8b47-0a133c518b25"
AND status IN (10346, 10014, 10015, 10027, 10019)
ORDER BY priority DESC, key ASC
```

Substitute a resolved assignee or add a project restriction when requested.
There is no minimum age or inactivity threshold. Do not fetch changelogs or use
`updated`, `created` or `status CHANGED` to exclude fresh tasks. This is a current
pending-work list, not the scheduler's stale-status report.

## Report

Respond briefly in Polish with the person's name and total count, then one line
per issue: `[KEY](direct issue URL) — title — exact status — priority`.
Use the real site's `/browse/KEY` links. Keep the result in the conversation;
do not send it to Slack or other people. Do not claim that a pending issue is
stale or blocked without evidence.

If the complete query is empty, say the person has no tasks in the selected
pending statuses. Report authentication, permission and pagination failures as
an incomplete or failed check, never as zero tasks. Only claim completeness for
the issues visible to the integration account.

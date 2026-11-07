---
name: kineti-pending-tasks
description: "Trigger only when the user explicitly invokes this skill to list a person's pending Kineti Jira work."
---

# Pending Kineti tasks

## Scope

Report current assigned pending/fix/review work here, read-only. Do not change
issues or schedules, or notify others.

## Workflow

**Defaults**

- Site: https://jirahub.atlassian.net
- Cloud ID: `2f501818-4c78-481c-bab0-7c3372756f44`
- Assignee: Krystian Sztadhaus, `712020:ddb6b06c-54d0-4d0c-8b47-0a133c518b25`.
  “Me” means this configured person, not a different integration account.
- Scope: accessible projects on this site unless narrowed; no other companies.
- Statuses: To Fix (10346), QA Testing Pending (10014), PR Merge Pending (10015),
  In Review (10027), PR Review (10019).

Resolve other requested people to exact Jira account IDs; ask on ambiguous matches.
Use status IDs. If rejected or other statuses are requested, resolve site/project
metadata, including newly confirmed Pending statuses. Do not infer meaning from
categories or broaden to all review/unfinished/blocked work.

```jql
assignee = "712020:ddb6b06c-54d0-4d0c-8b47-0a133c518b25"
AND status IN (10346, 10014, 10015, 10027, 10019)
ORDER BY priority DESC, key ASC
```

Substitute resolved assignee/project restrictions when requested. Include every
issue type and fresh tasks; do not use age filters, changelogs or
updated/created/status CHANGED restrictions. This is not a stale-status report.

**Data-only access**

Use Atlassian executeRead operation `searchJiraIssuesUsingJql`, not the same-named
dedicated tool that produces previews. Consult connector documentation if needed;
report unavailability instead of switching to that tool.

Pass top-level cloudId and responseFields:
`["issues.key", "issues.fields.summary", "issues.fields.status.name", "isLast", "nextPageToken"]`.
Inputs: jql, fields `["key", "summary", "status", "assignee", "priority"]`,
maxResults 100 and nextPageToken for subsequent pages. Read returned data until
isLast; deduplicate keys and count the complete visible result.

Return only required fields, pagination and errors from orchestration, not full
MCP results, UI metadata or resources. Do not render cards or switch tools to
preview/validate the report. Use available data-only/no-preview modes; disclose unavoidable host
previews instead of claiming suppression.

## Result

Reply in Polish with name and count. Group by exact status in this order:
In Review, PR Review, To Fix, QA Testing Pending, PR Merge Pending. Append other
explicitly selected statuses and omit empty groups. Preserve query priority/key
order within groups. List each issue once as
`[KEY](https://jirahub.atlassian.net/browse/KEY) — title`,
without repeated status or priority. Use plain Markdown, without cards, widgets or
interactive lists.

Do not infer staleness or blockers. Only a complete empty query means no tasks in
selected statuses; auth/permission/pagination failures mean incomplete/failed,
never zero. Completeness covers only issues visible to the integration account.

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

1. Identify the authenticated GitHub user (`gh api user --jq .login`). Determine
   the Kineti repository scope and Slack destination from the invocation or
   established project context. Ask for missing details; in unattended runs,
   report the blocker without sending.
2. Fetch the user's open PRs in that scope, including all pages. Select PRs that
   are ready for review and still need it, checking reviews for the current
   revision. Skip drafts, closed or merged PRs, and PRs with required approvals,
   unless the user asks otherwise.
3. Send one concise Slack message with each PR's title, repository and link.
   Use Polish unless the team has established another language. If nothing
   needs review, send nothing and report that result.
4. Report the number of included PRs and the sent message link, if available.
   Report missing access or incomplete data honestly. After an uncertain send,
   check channel history before retrying to avoid duplicates.

Use confirmed account mappings below when a reviewer needs to be identified.
Mention people only when requested or consistent with established team practice;
do not ping everyone in the table or use `@channel` / `@here`. Do not guess
missing mappings. This workflow does not create or change schedules.

## Kineti contacts

Initial contact directory, not a confirmed roster of the user's team. Names and
Slack details were read from Kineti Slack on 2026-11-01; GitHub mappings were
checked against `kineti1` membership and profile names. `Unconfirmed` means the
GitHub mapping still needs verification. Refresh mappings if accounts change.

| Name | GitHub ID (login) | Slack ID | Email |
| --- | --- | --- | --- |
| Adam Budziak | `adambudziak` | `U081WMVNDRV` | adam@kineti.nl |
| Jakub Jurczak | `jakujur` | `U08GTSN023F` | jakub.j@kineti.nl |
| Kacper | Unconfirmed | `U0C8AREURDH` | kacper@kineti.nl |
| Konrad Świerczyński | `KonradSwierczynski` | `U08EQAE4T7Y` | konrad@kineti.nl |
| Maciej Artymowicz-Olejnik | `macoley` | `U08DZSWLN6M` | maciej@kineti.nl |
| Oleh Botsiun | `Boto1` | `U082BATBUF4` | oleh@kineti.nl |
| Tonny Phan | `tonny-kineti` | `U014JEH4ZA4` | tonny@kineti.nl |

## Example message

Illustrative titles and links; replace them with actual PR data before sending.

> Cześć! Moje PR-y czekają na review:
> - [Fix duplicate form submissions](https://github.com/ORG/REPO/pull/123) — ORG/REPO
> - [Improve validation messages](https://github.com/ORG/REPO/pull/124) — ORG/REPO
>
> Dzięki za spojrzenie!

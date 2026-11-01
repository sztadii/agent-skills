# Kineti skills

[Back to main README](../../../README.md)

Workflows specific to Kineti. Invoke skills explicitly with `$skill-name` in
Codex or `/skill-name` in Claude Code. Another skill can also explicitly invoke
them.

## Skills

- [kineti-pr-reminder](kineti-pr-reminder/SKILL.md) — Checks your open Kineti PRs and sends one Slack reminder for PRs that still need review.

## Installation

Install all skills, including generic, Kineti and other companies:

```sh
npx skills add sztadii/agent-skills -g --skill '*'
```

See the [main installation instructions](../../../README.md#installation).

To install only the Kineti reminder instead:

```sh
npx skills add sztadii/agent-skills -g --skill kineti-pr-reminder
```

## Examples

Use an established repository scope and Slack channel:

> $kineti-pr-reminder Sprawdź moje aktualne PR-y Kineti i wyślij zespołowi przypomnienie o review na ustalonym kanale Slack.

Provide the repository scope and destination explicitly when needed (replace the
placeholders with actual values):

> $kineti-pr-reminder Sprawdź moje PR-y w repozytorium <organizacja>/<repozytorium> i wyślij przypomnienie na kanał Slack <ID-kanału>.

Invoking the skill authorizes sending the reminder. It skips drafts and PRs that
already have the required approvals, and sends nothing if no PR needs review.
GitHub and Slack access are required; an ambiguous repository scope or channel
must be resolved before sending.

The skill performs one check per invocation. Scheduling it requires a separate
request that explicitly names `$kineti-pr-reminder`.

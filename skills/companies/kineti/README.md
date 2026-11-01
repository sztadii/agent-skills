# Kineti skills

[Back to main README](../../../README.md)

Workflows specific to Kineti. Invoke skills explicitly with `$skill-name` in
Codex or `/skill-name` in Claude Code. Another skill can also explicitly invoke
them.

## Skills

- [kineti-pr-reminder](kineti-pr-reminder/SKILL.md) — Checks `sztadii`'s open PRs in `kineti1` and sends each teammate a Slack DM for PRs still awaiting their approval.

## Examples

> $kineti-pr-reminder Check my current Kineti PRs and send review reminders to Oleh, Michał G and Jakub.

Invoking the skill authorizes sending reminders to the configured Slack DMs.
Each teammate receives their own list of fully ready PRs without their valid
approval, only on their working days in `Europe/Warsaw`. Drafts, unresolved
review threads, merge conflicts, and pending or failed CI prevent a reminder.
No message is sent for an empty list. GitHub and Slack access are required.

The skill performs one check per invocation. Scheduling it requires a separate
request that explicitly names `$kineti-pr-reminder`.

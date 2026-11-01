# Kineti skills

[Back to main README](../../../README.md)

Company-specific workflows for Kineti PR reviews, dependency updates and backend contract updates.

Invoke skills explicitly with `$skill-name` in Codex or `/skill-name` in Claude
Code. Another skill can also explicitly invoke them.

## Skills

- [kineti-pr-reminder](kineti-pr-reminder/SKILL.md) — Sends private Slack DMs requesting Kineti PR reviews.
- [kineti-fe-gdk-update](kineti-fe-gdk-update/SKILL.md) — Updates `@geodome/gdk` in a Kineti frontend consumer project, including required code and translation adaptations and verification. Leaves uncommitted changes for review.
- [kineti-fe-types-sync](kineti-fe-types-sync/SKILL.md) — Regenerates backend types in `drones-frontend` or `gdk-frontend`, adapts code and mocks, and verifies the local changes. Leaves uncommitted changes for review.

## Examples: single skill

> $kineti-pr-reminder

Reminders are sent only as private Slack DMs. By default, each eligible teammate
from the skill's team table receives a separate message.

> $kineti-pr-reminder Wyślij tylko Michałowi.

> $kineti-fe-gdk-update

> $kineti-fe-types-sync

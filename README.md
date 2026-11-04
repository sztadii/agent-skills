# Agent skills

Skills for coding agents to implement changes, streamline code, verify behavior
and review PRs, with separate company-specific workflows.

## Installation

Install every skill for Codex and Claude Code with one command — generic, Kineti
and any other companies added to this repository:

```sh
npx skills add sztadii/agent-skills -g --skill '*' --agent codex claude-code
```

The installer discovers skills in nested directories. The folder split is only
for organization; no separate installation per collection is needed.

## Skill collections

- [Generic skills](skills/generic/README.md) — Shared workflows, skill descriptions and usage examples.
- [Kineti skills](skills/companies/kineti/README.md) — Kineti workflows, installation and usage examples.

## Organization

- `skills/generic/<skill-name>/` — reusable skills shared across projects and companies.
- `skills/companies/<company>/<skill-name>/` — workflows specific to one company.

Add each new company under `skills/companies/`, with its own README linked above.
Prefix company-specific skill names with the company name (for example,
`kineti-pr-reminder`) so installed skills remain distinguishable. Directory
organization does not change invocation names.

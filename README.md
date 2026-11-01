# Agent skills

Skills for coding agents to implement changes, simplify code, verify behavior
and review PRs, with separate company-specific workflows.

All skills require explicit invocation: `$skill-name` in Codex, `/skill-name` in
Claude Code, or an explicit call from another skill. Mentions, examples and
matching tasks alone do not trigger them.

Codex disables implicit invocation through `agents/openai.yaml`. Claude Code
uses the descriptions and instructions to limit invocation while allowing skills
to call each other.

## Installation

Install every skill with one command — generic, Kineti and any other companies
added to this repository:

```sh
npx skills add sztadii/agent-skills -g --skill '*'
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

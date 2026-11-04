# Repository instructions

## Refresh installed skills after pushing main

After every successful push to `origin/main` in `sztadii/agent-skills`, immediately
reinstall all repository skills globally for both Codex and Claude Code. This is
a required part of completing the push; do not wait for a separate user reminder.

Run from the repository root:

```sh
npx skills add sztadii/agent-skills -g --skill '*' --agent codex claude-code --yes
```

Install from GitHub after the push so the installed skills reflect the published
version. Wait for the installer to finish and check its exit status. If it fails,
resolve the failure and retry when possible; otherwise report that the push
succeeded but the installed skills are not confirmed up to date.

Before reporting completion, verify that every repository skill is installed for
both agents and that its installed files match the pushed version. Include the
installation result in the final response. Keep global installation artifacts
and `.skill-lock.json` out of commits, as specified by `.gitignore`.

## Skill structure

Every repository `SKILL.md`, including new skills, must have YAML frontmatter
with `name` and `description`, one H1 title, and exactly these H2 sections in order:

1. `Scope` — purpose, inputs, permissions and boundaries.
2. `Dependencies` — required skills and the shared check below. If none are
   required, use exactly `No skill dependencies`.
3. `Workflow` — execution, verification, stopping conditions and relevant
   supporting references.
4. `Result` — expected output, evidence and incomplete/blocked outcomes.

Use bold labels within sections for skill-specific stages; do not add other
heading levels. Keep attribution with the relevant content. Preserve behavior
when reorganizing instructions.

For skills with dependencies, list them as `Required:` with links, followed by
this exact paragraph. Keep workflow-specific rules outside this shared check:

```text
Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.
```

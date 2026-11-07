---
name: full-simplify
description: "Have a council of six agents find a simpler, less invasive version of a change with the same behavior and apply the best proposals to the working tree. Trigger only when the user explicitly invokes this skill."
---

# Full Simplify

## Scope

Make a change smaller and simpler without changing what it does. The subject is
a diff: local changes by default, or a branch, commit range or PR the user
names. A Moderator gives three fixed categories to six Analysts, one Codex and
one Claude per category, and turns their reports into validated proposals:

1. Smaller diff: a simpler or less invasive solution, fewer touched files and
   lines, no incidental churn.
2. Redundant tests: tests or scaffolding the change adds that duplicate
   coverage or test implementation details.
3. Reuse: new code that duplicates helpers, components or patterns the
   codebase already has and could simply reuse.

Each role's model, effort, responsibilities and output live in its reference
file; use them as written unless the user overrides a setting:

- [Moderator](references/moderator.md)
- [Analyst](references/analyst.md)

Invocation authorizes reading code, starting the agents above and editing the
working tree to apply the validated proposals without asking for approval. The
agents are read-only. It does not authorize commits, pushes, PR comments or
publication; leave the result uncommitted. Honor narrower user constraints and
pass them to every agent.

## Workflow

Agents start with no parent history, so every brief must be self-contained:
include the subject, constraints, relevant context and the absolute path of the
role's reference file, and tell the agent to read it first. The Moderator also
needs the absolute path of the Analyst's file to pass it down. In T3 Code, check
`orchestrator_capabilities` once per thread and start agents with
`delegate_task` using the provider, model and options from the reference file;
follow each task's `next` without polling.

1. Identify the subject: the base, the diff (staged, unstaged and untracked
   task files for local changes), the intended behavior, repository
   instructions and conventions. Resolve verifiable repository facts yourself;
   ask the user only if the subject is genuinely unclear. An empty diff ends the
   workflow with nothing to simplify.
2. Start the Moderator (`role: "review"`, shared checkout) with that context.
   It runs the six Analysts in parallel according to its reference file and
   returns validated proposals. Do not start the Analysts yourself.
3. Apply every validated proposal in the working tree without asking the
   user, preserving unrelated work. Where proposals conflict, the Moderator has
   already chosen the best one. If there are no proposals, end the workflow.
4. Run fast checks scoped to the changed files: lint, typecheck and related
   unit tests. If a proposal breaks behavior or a check, undo that proposal and
   report it instead of reworking it. Do not commit or push the result.

## Result

Lead with whether the change got smaller and whether behavior is preserved.
List each proposal with its category, location and status: applied or undone
with the reason. Then give the diff size before and
after, changed paths, checks with observed results, verification gaps and any
unavailable Analyst. If the workflow stopped, say
which stage completed and what blocks the rest.

---
name: full-council
description: "Analyze code or a problem described in chat with a council of six agents across three categories, summarized in one synthesized report. Trigger only when the user explicitly invokes this skill."
---

# Full Council

## Scope

Get a multi-model analysis of a subject: code (files, a diff, a PR or a module)
or a problem described in the conversation. A Moderator picks three categories
for the subject, gives each to two Analysts, one Codex and one Claude, and turns
their six reports into one synthesized report. The main agent (you) only
orchestrates: it gathers context, starts the Moderator and relays its report.
It does not analyze the subject itself.

Each role's model, effort, responsibilities and output live in its reference
file; use them as written unless the user overrides a setting:

- [Moderator](references/moderator.md)
- [Analyst](references/analyst.md)

Invocation authorizes reading code and context and starting the agents above.
Every agent is read-only: no file edits, commits, pushes, PR comments or other
publication. Honor narrower user constraints and pass them to every agent.

## Workflow

Agents start with no parent history, so every brief must be self-contained:
include the subject, constraints, relevant context and the absolute path of the
role's reference file, and tell the agent to read it first. The Moderator also
needs the absolute path of the Analyst's file to pass it down. In T3 Code, check
`orchestrator_capabilities` once per thread and start agents with
`delegate_task` using the provider, model and options from the reference file;
follow each task's `next` without polling. Title every agent
`<model> - <role>` (`title` in `delegate_task`, `description` for a native
subagent), with the model's family and version (`claude-opus-5-5` →
`Opus 5.5`, `gpt-6.1-sol` → `Sol 6.1`), for example `Astra 6 - Moderator`.

1. Identify the subject and the user's question. For code, collect the relevant
   paths, revision or diff, repository instructions and conventions. For a chat
   problem, restate it with its goals, constraints and known facts from the
   conversation. Resolve verifiable repository facts yourself; ask the user only
   if the subject or question is genuinely unclear.
2. Start the Moderator (`role: "review"`, shared checkout) with that context and
   any categories the user requested. It picks the categories, runs the six
   Analysts in parallel according to its reference file and returns the
   report. Do not start the Analysts yourself.
3. Wait for the Moderator's report. If it returns a blocking question, relay it
   to the user and send the answer to the Moderator's thread.

## Result

Relay the Moderator's report verbatim: a synthesis of the six analyses rather
than a list of them. Add one line naming the three categories and any agent
that was unavailable. If the Moderator was unavailable or the workflow stopped,
say which stage completed and what blocks the rest.

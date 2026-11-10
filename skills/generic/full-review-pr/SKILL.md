---
name: full-review-pr
description: "Check out a PR locally, have a council of eight agents look for critical bugs, unnecessary duplication, inconsistencies with project rules and unmet Jira requirements, and publish only the comments the user accepts. Trigger only when the user explicitly invokes this skill."
---

# Full Review PR

## Scope

Review one PR once and publish only the comments the user accepts. The subject
is the PR named by the user or inferred from context. A Moderator gives four
fixed categories to eight Analysts, one Codex and one Claude per category, and
turns their reports into validated findings:

1. Critical bugs: defects, regressions, data loss, security holes and broken
   contracts the PR introduces.
2. Duplication: new code that repeats existing helpers, components, patterns
   or other code in the PR without need.
3. Project rules: inconsistencies with the repository's instructions,
   conventions and patterns already in use.
4. Jira requirements: whether the PR meets the linked Jira task's
   requirements and acceptance criteria. Only when the PR relates to a Jira
   task; otherwise this category and its two Analysts are skipped.

Each role's model, effort, responsibilities and output live in its reference
file; use them as written unless the user overrides a setting:

- [Moderator](references/moderator.md)
- [Analyst](references/analyst.md)

Invocation authorizes checking out the PR branch locally, reading code, the PR
and its Jira task, and starting the agents above. Publishing a comment requires
the user's acceptance of that comment. The agents are read-only. Do not fix
code, commit, push, approve, request changes, resolve threads, merge or monitor
the PR. Never discard local work. Honor narrower user constraints and pass
them to every agent.

## Workflow

Agents start with no parent history, so every brief must be self-contained:
include the subject, constraints, relevant context and the absolute path of the
role's reference file, and tell the agent to read it first. The Moderator also
needs the absolute path of the Analyst's file to pass it down. In T3 Code, check
`orchestrator_capabilities` once per thread and start agents with
`delegate_task` using the provider, model and options from the reference file;
follow each task's `next` without polling. Reference files name a model family
(`Opus`, `Fable`, `Sol`, `Astra`), not a version: resolve each family to the
newest model ID of that family listed by `orchestrator_capabilities` (use its
`search` when the short list omits it) and pass the family-to-ID mapping in
every brief, so nested agents use the same IDs. Report a family with no model
instead of substituting another. Title every agent
`<model> - <role>` (`title` in `delegate_task`, `description` for a native
subagent), with the model's family and version (`claude-opus-5-5` →
`Opus 5.5`, `gpt-6.1-sol` → `Sol 6.1`), for example `Astra 6 - Moderator`.

**Review**

1. Identify the PR; ask if it is missing or ambiguous. If it is merged or
   closed, report that and stop. Read its title, description, base, head,
   diff, repository instructions and conventions.
2. Check out the PR branch locally (`gh pr checkout <number>`) and record the
   head SHA. If the working tree has uncommitted changes, stop and ask the user
   instead of stashing or discarding them.
3. Look for a Jira key in the PR title, description, branch name and commits.
   If one is found, read the issue (description, acceptance criteria, relevant
   comments and subtasks) through the available Atlassian tools. If the issue
   cannot be read, tell the user and continue without category 4.
4. Start the Moderator (`role: "review"`, shared checkout) with the PR, head
   SHA, diff, repository rules and Jira requirements if any. It runs the
   Analysts in parallel according to its reference file and returns validated
   findings. Do not start the Analysts yourself.

**Ask**

If there are no findings, report that and stop without publishing. Otherwise
fetch the PR's review comments, review bodies and discussion, including
resolved and outdated threads. Set aside each finding an existing comment by
anyone already raises, keeping it for the report with a link to that comment.
A shared topic or location alone is not a duplicate, and a problem that recurs
after its thread was resolved is not covered. If no findings remain, report
that and stop. Draft one comment per remaining finding as in
[PR comments](references/pr-comments.md)
and show each with a short ID (`C1`, `C2`, …), its category, location, exact
text and evidence. Ask the user to accept all, some or none, and wait. Never
publish rejected or unanswered drafts.

**Publish**

1. Right before publishing, fetch the existing comments again, since others may have commented
   during the review. Skip each accepted comment now covered by an existing
   one, as defined under **Ask**, and keep it for the report with a link.
2. Publish the remaining accepted comments, with any user edits, in one
   COMMENT review against the recorded head SHA as in
   [PR comments](references/pr-comments.md). If the head moved, recheck that
   each comment still applies and tell the user about dropped ones.
3. Verify the published bodies and URLs, then stop.

## Result

Report the reviewed PR and head SHA, the checked-out branch, the Jira task or
that none was linked, and the categories covered with any unavailable Analyst.
List each finding with its ID, category, location and status: published (with
URL), rejected, or skipped because an existing comment already covers it
(with a link to it). State that no approval or change request was submitted and
the PR is not monitored. If the workflow stopped, say which stage completed and
what blocks the rest.

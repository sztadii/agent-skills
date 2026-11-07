---
name: full-self-improvement
description: "Review the T3 Code threads created on the previous working day for repository skills that were misinterpreted, made avoidable mistakes or ran unnecessary commands, judging by the human's reactions, and apply targeted fixes to the skill files. Trigger only when the user explicitly invokes this skill."
---

# Full Self Improvement

## Scope

Learn from yesterday's work. The subject is every T3 Code thread created on the
previous working day in which a skill from this repository ran. Read those
threads and look for places where a skill:

1. was interpreted differently from what the human meant or what its text
   intended;
2. made a mistake it could have avoided with clearer or more complete
   instructions;
3. ran a script, command, check or agent it did not need, or repeated one.

The human's reactions are the main evidence. A correction ("no", "that's
wrong", "I meant…"), the same instruction repeated or rephrased, an interrupt,
a rollback, a rejected tool call or permission request, or the human finishing
the task by hand all mark a problem. Silence or acceptance after a step marks it
as fine.

The default window is the previous working day in the local time zone, from
00:00 to 24:00: Friday when run on Monday (or on a weekend), otherwise the day
before. A date or range the user supplies replaces it. The skill is meant to
run on a weekday schedule, for example every Monday–Friday at 18:00.

Invocation authorizes reading T3 Code threads in every project and environment
and editing this repository's `SKILL.md`, `references/*` and
`agents/openai.yaml` files to apply the validated fixes without asking for
approval. Do the work yourself; start no subagents or delegates. It does not
authorize commits, pushes, PRs, messages in the analyzed threads or edits
outside this repository; leave the result uncommitted. Honor narrower user
constraints.

## Workflow

1. **Prepare.** Confirm the working directory is a checkout of this skills
   repository (it contains `skills/` with `SKILL.md` files) and its working
   tree is clean; otherwise stop, because fixes must not mix with unrelated
   work. List the repository's skill names from their frontmatter. Compute the
   window and state it with dates and the time zone. Outside T3 Code, without
   the thread tools, stop as blocked.
2. **Collect threads.** List threads with `t3_thread_list` (`allProjects`,
   including subagents) in every environment from `t3_environment_links`.
   Select threads by `createdAt` only: keep those created inside the window,
   regardless of when they were last updated. Pages are newest first; keep
   paging until a whole page was created before the window. Drop this run's
   thread and earlier runs of this skill. Read each remaining top-level
   thread's messages with small reads and keep those in which a repository
   skill was invoked (`$<skill>`, `/<skill>`, a skill tool call or a
   skill-defined agent title such as `Astra 6 - Moderator`). Attach each kept
   thread's subagent and delegate threads, whatever their own creation date,
   since that is where skills run their scripts and agents. No kept threads
   ends the workflow with nothing to analyze.
3. **Analyze.** Read each kept thread with `t3_thread_read`: `messages` for the
   conversation and `activity` for tool calls, commands, agent starts and their
   results. Keep reads small, page through and read only the part of a long
   thread around each skill run. For every human reaction from the Scope, find
   the agent action it answers and compare it with the skill's current
   `SKILL.md` and references. Also note scripts, installs, checks, agents or
   repeated commands that did not change the outcome, failed predictably or
   duplicated an earlier step, even without a reaction.
4. **Validate.** Keep a problem only if the thread shows it, the skill's
   instructions caused or allowed it, and the current text does not already
   prevent it (a skill may have changed since). Drop problems caused by the
   environment (unavailable provider, network, credentials), by the human
   changing their mind or by code outside the skills, and drop fixes that
   conflict with repository instructions or with design decisions the user
   has recorded as intentional. Merge duplicates across threads.
5. **Apply.** Make the smallest edit that prevents each validated problem:
   clarify an ambiguous instruction, add a missing condition or stopping rule,
   or remove or narrow a needless step. Preserve the skills' structure and
   every behavior unrelated to the problem, keep wording consistent with
   sibling skills, and update the generic or company README when a skill's
   summary changes. Do not add rules for one-off mistakes that the current
   text already forbids clearly.
6. **Verify.** Check that every edited `SKILL.md` keeps its frontmatter, one
   H1 and exactly the `Scope`, `Workflow` and `Result` H2 sections in order,
   that descriptions still contain the explicit-invocation sentence, that every
   `agents/openai.yaml` still sets `allow_implicit_invocation: false`, and that
   relative links resolve. Run the repository's formatter or checks if it has
   any. Undo an edit that fails and report it instead.

## Result

Lead with the window, the number of threads analyzed and how many problems were
found and fixed. For each problem give the skill, the thread link, the human's
reaction that revealed it (paraphrased, without secrets or personal data), the
category, the cause in the skill text and the change made, or why it was not
fixed. List dropped findings briefly with the reason, then the changed paths
and verification results. State that the changes are uncommitted. If nothing
needed fixing, say so with the threads checked. If the workflow stopped, say
which stage completed and what blocks the rest.

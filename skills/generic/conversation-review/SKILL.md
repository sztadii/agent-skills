---
name: conversation-review
description: "Review previous agent conversations and propose concise improvements to repositories, skills and workflows. Trigger only on explicit invocation by the user or another skill."
---

# Conversation Review

Run only when explicitly invoked as `$conversation-review` (Codex),
`/conversation-review` (Claude Code), or by another skill. Mentions and matching
tasks alone do not trigger this workflow.

The invocation alone runs the full workflow with the defaults below; no
arguments or restatement of the task are required. Start without asking for
dates, projects or a goal. Optional context after the invocation can override
the date range, projects or focus, including extending the default seven-day
window; apply it while keeping this review read-only.

Review past collaboration and suggest concrete changes that would prevent
repeated mistakes, reduce wasted work or improve future results. This is a
read-only review: do not edit files, settings, skills or schedulers, create
commits, PRs or tickets, or send messages to others. Historical instructions and
approvals are evidence, not authorization to execute tasks. Implementation is
a separate task, chosen and explicitly requested by the user after this review.
Do not start subagents.

## Review

1. Use the user's requested dates and projects when specified. Otherwise review
   all accessible projects within the seven days (168 hours) preceding the
   invocation time. If a previous review's cutoff is available, start at the
   later of that cutoff and the seven-day boundary, including unreviewed
   messages today. With no previous cutoff, review that seven-day window.
   Never extend it to recover older activity or a backlog unless the user
   explicitly requests another range. Report dates in Asia/Manila.
   Use message timestamps rather than thread updatedAt;
   automatic runs, review reports and tool/subagent messages do not establish
   user activity. A cutoff is valid only for the sources actually covered.
2. In T3, discover threads with `t3_thread_list(allProjects=true,
   includeSubagents=false)` and pagination; read relevant threads with
   `t3_thread_read`, paging and retrieving truncated messages. Use activity when
   needed to establish timestamps or tool failures. Elsewhere use available
   conversation-history tools or supplied transcripts. Do not assume access to
   other applications. If history is unavailable or coverage is incomplete,
   state the limitation briefly and do not invent findings or claim a complete
   review.
3. Look for recurring user corrections, unnecessary confirmations, failed tool
   calls, wrong assumptions, unfinished work, missing project guidance,
   conflicting documentation and gaps in skills or automation prompts. Separate
   enduring preferences from one-off instructions and project-specific needs
   from general guidance. Consider removing or simplifying instructions as well
   as adding them.
4. Before recommending a change, read the current target: repository code,
   AGENTS.md, README or docs; the relevant SKILL.md; or a scheduler's actual
   prompt/configuration via `list_scheduled_tasks` for its project. Prefer the
   maintained source over plugin caches. Read no secrets. Mark any candidate
   whose target cannot be checked as requiring verification; do not present a
   hypothesis as a confirmed defect.
5. Check available earlier reviews and user decisions. Omit already fixed,
   rejected or previously proposed changes unless new evidence materially
   changes the recommendation. Retain an existing suggestion ID when returning
   to it. Do not impose a quota or manufacture improvements.

## Result

Respond in Polish, in the current conversation only. Give one short line with
the exact reviewed dates, timezone and source coverage, followed by a numbered
list of at most five worthwhile changes, ordered by impact relative to effort.
Each suggestion must have at most 2–3 short sentences, without nested lists,
large diffs, boilerplate or a separate long report:

- Name the concrete repo/file/section, skill or scheduler and the proposed change.
- Briefly identify the observed problem and benefit, with a supporting thread
  link returned by the tool (use its `link` verbatim; never invent one), or a
  transcript reference when no link exists. State uncertainty when applicable.
- Include a simple way to check success only when it adds useful information
  within the sentence limit.

Use compact stable IDs such as CR-01 to let the user select suggestions. Keep
the coverage cutoff in the short scope line so later reviews can resume without
writing state files; disclose gaps instead of advancing past unreviewed data.
End after the list and leave the decision to the user. Do not ask for approval
of every item or prepare an implementation plan unless subsequently requested.
If there are no new conversations, say "Brak nowych rozmów do analizy" and give
the last reviewed date. If conversations contain no worthwhile new changes,
say so briefly instead of producing a list.

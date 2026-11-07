---
name: full-feature-flow
description: "Take a feature, fix or skill change from idea to verified working-tree code: agree on a plan with a Planner, have a Coder implement it and a Quality Leader check it for up to three rounds, then run the final checks. Trigger only when the user explicitly invokes this skill."
---

# Full Feature Flow

## Scope

Take a feature, fix or skill change from idea to working-tree code: a Planner,
helped by two parallel analysts, agrees on the plan with the user, and a Coder
implements it and requests verification from a Quality Leader. The Quality
Leader manages its agents according to its own instructions and returns a
verdict and validated findings. The Coder fixes findings in up to three rounds,
runs the final checks, prepares screenshots or recordings of the final version
when there is something visible to show and reports the results. The main agent
(you) only orchestrates: it gathers context, relays the Planner's questions and
plans, collects the user's approval and starts the Coder. It does not plan,
implement or review.

Each role's model, effort, responsibilities and output live in its reference
file; use them as written unless the user overrides a setting:

- [Planner](references/planner.md)
- [Feature Conflict Analyst](references/feature-conflict-analyst.md)
- [Impact Analyst](references/impact-analyst.md)
- [Coder](references/coder.md)
- [Quality Leader](references/quality-leader.md)
- [Code Reviewers](references/code-reviewer.md)
- [Critical Issue Reviewers](references/critical-reviewer.md)
- [User Flow Testers](references/user-flow-tester.md)

Invocation authorizes planning, editing the working tree and starting the
agents above. It does not authorize commits, pushes, PRs or publication; honor
narrower user constraints and pass them to every agent.

## Workflow

Agents start with no parent history, so every brief must be self-contained:
include the task, constraints, relevant context and the absolute path of the
role's reference file, and tell the agent to read it first. The Planner also
needs the paths of its analysts' files, and the Coder the paths of the
Quality Leader's and its agents' files, to pass them down. In T3
Code, check `orchestrator_capabilities` once per thread and start agents with
`delegate_task` using the provider, model and options from the reference file;
follow each task's `next` without polling. Title every agent
`<model> - <role>` (`title` in `delegate_task`, `description` for a native
subagent), with the model's family and version (`claude-opus-5-5` →
`Opus 5.5`, `gpt-6.1-sol` → `Sol 6.1`), for example `Sol 6.1 - Planner`.

**Plan**

1. Gather context: the user's request, relevant repository instructions, files
   and conventions, and known constraints. Resolve verifiable repository facts
   yourself.
2. Start the Planner (`role: "design"`, shared checkout) with that context.
   It runs its analysts according to its own reference file and returns
   exactly three questions at once. Do not start the analysts yourself.
3. Show the user all three questions together and wait for the answers. Send
   the answers to the Planner's thread (`t3_thread_send` to its child thread)
   so it keeps its context; it returns a plan of at most six concise sentences.
4. Show the user the complete plan and ask whether they approve it. If not,
   send their feedback to the same Planner thread and show the revised complete
   plan, also limited to six concise sentences. Repeat until the user explicitly
   approves. Never approve on the user's
   behalf or treat silence or unrelated replies as approval.
5. After approval, stop the Planner (`task_cancel` its task) and keep the
   approved plan verbatim. If the user cancels, stop the Planner and end the
   workflow.

**Build and test**

6. Start the Coder (`role: "implementation"`, shared checkout) with the
   approved plan verbatim, the gathered context, user constraints and the
   absolute paths of every reference file except the Planner's and its
   analysts'.
7. The Coder implements the plan and prepares access for testing: a running web
   app's URL, a built mobile app or an executable command or script for other
   changes. It then starts a Quality Leader for each round. The Quality Leader
   manages verification according to its own reference file and returns a
   verdict and validated findings. The Coder fixes them until a round passes,
   for at most three rounds. After a passing round or round three, even if
   findings remain disputed or open, it runs the final checks: the whole unit
   suite, e2e tests and build. If any fail, it fixes them and reruns only the
   failures first, then reruns all final checks once those pass. When the change
   has visible behavior, it then captures, or reuses from earlier rounds, a
   screenshot or 1080p 60 fps recording of the final version. It reports the
   results, media and unresolved disagreements. Do not start the Quality Leader
   or its agents yourself.
8. If the Coder returns a blocking question, relay it to the user and send the
   answer to the Coder's thread; the Coder starts over with implementation and
   the Quality Leader rounds. Otherwise wait for its final report.

## Result

Relay the Coder's report to the user, leading with whether the requested change
was fully implemented and whether any problems remain. Include the prepared
app's URL, artifact path or invocation, its readiness, required setup and
services left running for the user or a later agent, and the visual evidence's
absolute paths, captions, quality and source snapshot or its capture gap, so the
user can review it and `full-create-pr` can attach it. Then give the approved
plan summary, changed paths, delivered behavior, checks and results, rounds run
with each Quality Leader's agents, user flow results and findings, findings
fixed, findings declined with reasons and evidence, unresolved disagreements
after the third round, open findings, final check results and remaining risks.
If any agent was unavailable or the workflow stopped, state which stage
completed, what blocks the rest and the decision needed. Do not report the task
as done unless the Coder reports it done.

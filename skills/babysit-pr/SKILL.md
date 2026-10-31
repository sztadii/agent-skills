---
name: babysit-pr
description: Trigger this if the user says 'babysit-pr' or 'babysit pr', or another skill invokes it.
disable-model-invocation: true
---

# Babysit PR

Maintain the task PR until available CI and required reviews settle. Authorizes
in-scope fixes, conflict resolution, task pushes and autonomous replies to existing
feedback. Preserve draft/ready status; do not mark ready or merge.

1. Read head, target drift, checks, reviews and all unresolved threads, including
   outdated ones. Evaluate feedback against current source; explain false
   positives through [leaving-pr-comments](../leaving-pr-comments/SKILL.md).
   Read `.macroscope/check-run-agents/*.md` when present. CodeRabbit is advisory;
   evaluate its findings but do not wait for it.
2. Fix valid findings, CI failures and target drift/conflicts within task scope.
   Use [jira-status](../jira-status/SKILL.md) on actual implementation resumption
   and [simplify](../simplify/SKILL.md) when useful. Report infrastructure blockers
   and reject speculative scope expansion with reasons. Task rebases may use
   `--force-with-lease` only when authorized by the user or caller.
3. Run affected [proof](../proof/SKILL.md) and refresh
   [council](../council/SKILL.md) after behavior changes or substantive conflict
   fixes; harmless metadata changes need no new council round. Fix justified
   findings before publishing through [file-pr](../file-pr/SKILL.md), then update
   Jira. Never push unverified fixes; accepted exceptions cover only their
   specific gap. Preserve evidence, scope and review rounds across wakes.
4. Reply with evidence before resolving verified fixes or false positives. Resolve
   only after the reply succeeds; another human's thread needs permission.
5. If work still waits, use [watching](references/watching.md). Stop on closure,
   user request, a required decision or settled checks/reviews. A settled draft
   is success; checks requiring ready status are a limitation to report.
   Ask before closing an obsolete PR.

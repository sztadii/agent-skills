---
name: simplify
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Simplify

## Scope

Make task code simpler with the fewest worthwhile edits. Prefer a small, clear diff;
do not sacrifice readability to save lines. Work independently of other skills;
do not commit, push, publish comments or change PR status.

Use the user or caller's selected files or change. Otherwise infer the task diff
from context: staged, unstaged and untracked task files and task commits against
the actual base. Preserve unrelated work and stay in scope. An empty diff is a
valid no-op, not permission for repository cleanup.

## Dependencies

No skill dependencies

## Workflow

- Read repository instructions, relevant code, callers and tests. Follow established
  patterns; remove unnecessary indirection, duplication, speculative branches and
  incidental churn to improve readability and maintenance.
- Stop when scoped code is clear enough. Avoid unrelated dependency upgrades, API
  redesigns, broad formatting and edits made only to produce a diff.
- Preserve normal behavior and avoid significant regressions. A minor regression
  in an extremely unlikely edge case is acceptable if it materially simplifies
  code. Base this judgment on callers, contracts or tests, weighing likelihood
  and impact; rarity cannot justify serious consequences. Honor stricter caller
  or user constraints.
- Keep tests proportional to risk. Consolidate redundant task-added tests and
  scaffolding without losing meaningful coverage or required validation. Add
  tests for meaningful gaps only; avoid implementation-tied assertions.
- Run relevant checks and repository-required validation; exercise behavior when
  structural checks are insufficient. Reuse current evidence for untouched paths. Fix
  significant regressions from your edits or undo only those edits. Separate
  existing failures and blocked checks from regressions; unavailable verification
  does not prove preserved behavior.

Resolve routine choices autonomously. If ambiguity affects behavior or scope,
finish independent cleanup and report the decision needed.

## Result

Report changed paths, clarity gains, checks, observed results and verification
gaps. Disclose accepted edge-case regressions, why they are unlikely, their impact
and the simplification they enable. State if no worthwhile change was found.

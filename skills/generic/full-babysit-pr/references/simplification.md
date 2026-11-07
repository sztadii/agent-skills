# Simplification

Make the local fix diff simpler with the fewest worthwhile edits. Prefer a small,
clear diff; do not sacrifice readability to save lines. Do not commit, push,
publish comments or change PR status here.

Scope is the complete local fix diff for this round: staged, unstaged and
untracked task files against the PR head. Preserve unrelated work and stay in
scope. An empty diff is a valid no-op, not permission for repository cleanup.

- Read repository instructions, relevant code, callers and tests. Follow established
  patterns; remove unnecessary indirection, duplication, speculative branches and
  incidental churn to improve readability and maintenance.
- Stop when scoped code is clear enough. Avoid unrelated dependency upgrades, API
  redesigns, broad formatting and edits made only to produce a diff.
- Preserve normal behavior and avoid significant regressions. A minor regression
  in an extremely unlikely edge case is acceptable if it materially simplifies
  code. Base this judgment on callers, contracts or tests, weighing likelihood
  and impact; rarity cannot justify serious consequences. Honor stricter user
  constraints.
- Never cut validation at trust boundaries, error handling that prevents data
  loss, security or accessibility, whatever the edge case. Moved or merged code
  keeps them.
- Keep tests proportional to risk. Consolidate redundant task-added tests and
  scaffolding without losing meaningful coverage or required validation. Add
  tests for meaningful gaps only; avoid implementation-tied assertions.
- Run only fast checks scoped to changed files: static checks (lint, typecheck,
  formatting) and unit tests related to those files (for example `vitest related`
  or `jest --findRelatedTests`). Leave whole suites, e2e and browser checks to
  verification. Fix significant regressions from your edits or undo only those
  edits. Separate existing failures and blocked checks from regressions;
  unavailable verification does not prove preserved behavior.

Resolve routine choices autonomously. If ambiguity affects behavior or scope,
finish independent cleanup and report the decision needed.

Record changed paths, clarity gains, checks, observed results and verification
gaps. Disclose accepted edge-case regressions, why they are unlikely, their impact
and the simplification they enable, or that no worthwhile change was found.

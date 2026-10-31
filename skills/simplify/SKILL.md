---
name: simplify
description: Trigger this if the user says 'simplify', or another skill invokes it.
disable-model-invocation: true
---

# Simplify

Run when explicitly requested by the user or invoked by another skill.
Prefer the smaller, more consistent
PR when behavior, correctness, and readability are equally good. Leaving a
well-scoped change untouched is a valid outcome.

## Scope and candidates

Inspect `git status --short` and identify the user's change paths; preserve
unrelated working changes. Fetch the relevant remote when available. For a PR,
use its actual target branch and confirm the local revision matches the intended
PR head plus any task edits. Otherwise use the repository's default branch or
the user's specified base. Fix the merge-base SHA for the entire review. With
no remote, use the user's base, or HEAD for uncommitted changes only; ask if the
committed scope cannot be determined.

Review base to working tree, including untracked files, not just the latest
commit. Read nearby code and tests for conventions. Record the starting diff
size, including new files, and use the same scope and base for the final count.

Look for concrete opportunities in the changed code:

- Remove unnecessary abstractions, branches, options, and speculative handling.
  Reuse established patterns when they make the change easier to understand.
- Simplify test setup, fixtures, mocks, and duplication while retaining distinct
  behavioral cases, useful assertions, and regression coverage. Excess tests
  also add maintenance and review cost: avoid adding or retaining tests that
  provide no meaningful regression protection, especially those duplicating
  other tests or merely mirroring the implementation.
- Shorten added comments that repeat the code. Keep explanations of intent,
  constraints, and non-obvious decisions that future readers need.
- Avoid unrelated formatting, renames, refactors, and churn. Prefer a local
  change over introducing a new mechanism for a single use.

Do not optimize line count by compressing code, hiding complexity in helpers,
moving code elsewhere, weakening tests, or removing necessary validation and
error handling. Existing conventions matter more than personal style.

## Parallel review and simplification

Optimize elapsed time even at higher token cost. Launch multiple independent,
read-only agents concurrently, within the harness's concurrency limit:

- Code structure: unnecessary abstractions, branches, and duplication.
- Tests: redundant setup and cases, preserving meaningful regression protection.
- Repository fit: existing patterns and reusable implementations nearby.

For larger diffs, divide independent modules among additional agents as capacity
allows. Give each agent the checkout, fixed base, paths, user intent, constraints,
and current check results. Ask for concrete smaller alternatives, file/line
references, and the behavior or coverage to preserve. Agents must not edit,
publish, or start more agents. Use the current model by default; honor any
explicit model choice and the harness's delegation rules. If no model was
specified and agents are unavailable, report that limitation and perform the
same passes yourself; do not claim independent review. If a requested model
is unavailable, report it and ask how to proceed rather than substituting a
model or a self-review.

Keep the reviewed files stable until the agents finish. Evaluate and deduplicate
their findings, then apply justified changes yourself. Challenge proposals that
obscure intent, weaken coverage, or expand scope. Run checks appropriate to the
changed behavior and tests. Request a focused independent re-check only for
substantive unresolved questions or risky edits; do not repeat clean reviews
or start new rounds for wording and formatting. Stop when no worthwhile
simplifications remain; report persistent disagreements instead of looping.

Confirm the original task still works and compare against the fixed base. A
smaller diff is a preference, not a quota. Report simplifications, deliberately
retained complexity, checks, review limitations, and diff size before/after.
This skill alone does not authorize committing, pushing, opening a PR, changing
draft status, or merging.

---
name: simplify
description: Simplify the current change for easier review while preserving quality and repository conventions. Use only when the user explicitly requests the simplify skill.
disable-model-invocation: true
---

# Simplify

Run only on the user's explicit request. Prefer the smaller, more consistent
PR when behavior, correctness, and readability are equally good. Leaving a
well-scoped change untouched is a valid outcome.

## Scope and candidates

Use the scope and base-selection steps from [sol-loop](../sol-loop/SKILL.md).
Review the whole proposed change against that fixed base, including untracked
files, not just the latest commit. Read nearby code and tests for conventions.
Preserve unrelated working changes. Record the starting diff size; include
new files when comparing the final result.

Look for concrete opportunities in the changed code:

- Remove unnecessary abstractions, branches, options, and speculative handling.
  Reuse established patterns when they make the change easier to understand.
- Simplify test setup, fixtures, mocks, and duplication while retaining distinct
  behavioral cases, useful assertions, and regression coverage.
- Shorten added comments that repeat the code. Keep explanations of intent,
  constraints, and non-obvious decisions that future readers need.
- Avoid unrelated formatting, renames, refactors, and churn. Prefer a local
  change over introducing a new mechanism for a single use.

Do not optimize line count by compressing code, hiding complexity in helpers,
moving code elsewhere, weakening tests, or removing necessary validation and
error handling. Existing conventions matter more than personal style.

## Discuss and simplify

Use [sol-loop](../sol-loop/SKILL.md) with one read-only Sol reviewer and its
three-round budget, delegation, and availability rules. Add the user's goal,
the relevant repository patterns, and the criteria above to the review brief.
Ask for concrete smaller alternatives and what behavior or coverage each must
preserve. Useful simplifications within this change are in scope, even when
they are not correctness bugs; do not automatically dismiss them as nits.
Do not invoke council unless the user explicitly chooses it instead.

Evaluate each suggestion yourself. Apply the smallest justified simplification;
challenge suggestions that obscure intent, lose coverage, or expand scope.
Use the next review round for unresolved substantive tradeoffs and changes
that need verification, following sol-loop's rules. Do not edit while the
reviewer is running. If no Sol route is available, report that limitation;
do not claim an independent review or silently substitute another model.

Run checks appropriate to the changed behavior and tests. Confirm the result
still meets the original task, then compare it with the same base. A smaller
diff is a preference, not a quota: keep necessary code and tests. Stop when
there are no worthwhile simplifications left or the review budget is exhausted;
report unresolved disagreements rather than forcing agreement.

Report what was simplified, what was deliberately retained and why, checks,
review outcome, and diff size before/after. This skill alone does not authorize
committing, pushing, opening a PR, changing draft status, or merging.

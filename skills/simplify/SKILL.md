---
name: simplify
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Simplify

Run only when the user explicitly invokes `$simplify` (Codex) or
`/simplify` (Claude Code), or another skill explicitly instructs you to invoke
this skill.
Mentions, examples and matching tasks alone do not trigger this workflow.

Produce clearer code and a verified local diff that a human can easily review.
Take a fresh look at the complete change, especially complexity introduced during
AI-assisted implementation. Prefer the smallest coherent task diff when it
preserves or improves code quality, behavior and meaningful verification; do not
trade readability for fewer changed lines or compress code merely to shrink a diff.
This skill works on its own; it needs no PR, council review, proof skill or
full-send run. It does not commit,
push, publish comments or change PR status.

## Scope

Use the files or change identified by the user or caller. Otherwise inspect the
current task diff, including staged, unstaged and untracked task files and task
commits against the actual base. Infer scope from the conversation and repository
before asking. An empty diff is a valid no-op, not permission to clean up the repo.
Preserve unrelated changes and existing public behavior.

## Work

1. Read the relevant code, callers, tests and repository instructions. Look for
   unnecessary indirection, duplicate logic, speculative branches and departures
   from established patterns. Prefer edits that reduce the effort of understanding
   or changing the code. Read the task diff against its actual base from a human
   reviewer's perspective: remove incidental churn and unnecessary additions so
   the purpose of the change is easy to follow. Fewer lines alone is not a reason
   to refactor.
2. Apply worthwhile changes directly. Preserve validation, error handling,
   compatibility and meaningful regression coverage. Avoid unrelated dependency
   upgrades, API redesigns and broad formatting churn.
   Keep tests proportional to the behavior and risk. Prefer a few meaningful
   behavioral or regression checks over redundant cases, assertions tied to
   implementation details or elaborate test scaffolding. Consolidate or remove
   unnecessary tests introduced by the task when doing so preserves useful
   coverage; do not weaken required validation or remove valuable tests just to
   reduce the diff. Add tests only where they close a meaningful verification gap.
3. Run checks relevant to the edits and repository-required validation. Exercise
   behavior where structural checks are insufficient. Reuse current evidence for
   untouched paths; do not require a separate verification workflow. If an edit
   introduces a regression, fix it or undo only that edit.

Make routine implementation choices autonomously. If a genuine ambiguity would
change behavior or scope, complete the independent cleanup first and report the
specific decision needed. Do not invent a simplification to produce a diff.

## Result

Return changed paths, why the code is clearer, checks and observed results, and
any remaining verification gap. State when no worthwhile change was found.
The caller may continue delivery; standalone use ends here.

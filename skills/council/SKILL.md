---
name: council
description: Trigger this if the user says 'council', or another skill invokes it.
disable-model-invocation: true
---

# Council

Return independent read-only review and a verdict. Neither council nor delegates
edit, run fix loops, publish comments or update PRs. The caller handles fixes,
refreshed proof and publication; standalone use reports findings.

1. Honor user-selected members; otherwise use current flagship models from three
   providers with high reasoning. Report unavailable members and ask; never
   silently substitute. In T3 Code use `orchestrator_capabilities` then
   `delegate_task`; elsewhere use harness delegation. Follow their waiting rules.
2. Freeze the exact head, local/untracked diff and actual base. Keep reviewed
   files stable. Give reviewers intent, repo guidance and evidence; assess
   correctness, regressions, integration, simplicity and meaningful tests.
   Delegates must not edit, publish or delegate.
3. Verify and deduplicate findings. Return revision, findings, each round's
   reviews/scores out of 10 (missing = “no score”) and a verdict satisfied only
   when all members accept the final diff.

Each re-review is a new round; retain previous rounds and unresolved objections.
Never invent scores, hide disagreements or expand scope for speculative advice.

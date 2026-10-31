---
name: council
description: Trigger this if the user says 'council', or another skill invokes it.
disable-model-invocation: true
---

# Council

Return independent review and a verdict. Read-only: neither council nor its
delegates edit code, run fix loops, publish comments or update PR descriptions.
The caller applies fixes, refreshes proof, requests another round and publishes
results. Standalone invocation reports findings to the user.

```ts
async function council(scope, evidence, previousRounds = []) {
  const members = await resolveMembers({ providers: 3, reasoning: "high" });
  if (!members.available) return { blocked: members.reason }; // Ask; never silently substitute.
  const snapshot = await freezeReviewScope(scope); // Exact head + local/untracked diff, fixed base.
  const reviews = await parallelReadOnlyReviews(members, {
    snapshot, evidence, intent: scope.intent, repositoryGuidance: scope.guidance,
    focus: ["correctness", "regressions", "integration", "simplicity", "meaningful tests"],
  });
  return {
    revision: snapshot.revision,
    satisfied: allMembersSatisfiedWithFinalDiff(reviews),
    findings: await verifyAndDeduplicateFindings(reviews),
    rounds: [...previousRounds, reviews], // Scores out of 10; absent score = "no score".
  };
}
```

Honor user-selected members; otherwise resolve current flagship models from
three providers. In T3 Code use `orchestrator_capabilities` then `delegate_task`;
elsewhere use harness delegation tools. Follow their waiting rules. Delegates
must not edit, publish or delegate. Keep reviewed files stable until completion.
Each re-review is a new round; carry unresolved objections forward. Never invent
scores, hide disagreements or expand scope to satisfy speculative suggestions.

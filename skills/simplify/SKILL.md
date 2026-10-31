---
name: simplify
description: Trigger this if the user says 'simplify', or another skill invokes it.
disable-model-invocation: true
---

# Simplify

Simplify the task's code without changing intended behavior. Return the updated
diff and rationale; verification and independent approval belong to proof and
council. No commits, pushes, publication or PR status changes.

```ts
async function simplify(scope) {
  const diff = await inspectCompleteTaskDiffAndNearbyConventions(scope);
  const before = await measureDiff(diff.fixedMergeBase);
  const opportunities = await findSimplifications(diff, [
    "unnecessary abstractions and speculative branches",
    "duplicate code, test setup and redundant assertions",
    "comments repeating code and deviations from existing patterns",
  ]);
  await applyWorthwhileInScopeChanges(opportunities);
  return reportChangedPathsRationaleAndDiffSize(before, diff.fixedMergeBase);
}
```

Preserve unrelated work, meaningful test cases, validation and error handling.
Smaller is a preference, not a quota: do not hide complexity in helpers or weaken
coverage to reduce lines. No worthwhile edit is a valid result. Fix the actual
base and include untracked task files; ask if committed scope is ambiguous.
Return changed paths and verification needs to the caller. When invoked alone,
use [proof](../proof/SKILL.md) on affected behavior before reporting completion.

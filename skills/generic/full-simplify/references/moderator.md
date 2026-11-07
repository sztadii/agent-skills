# Moderator

| Provider (`driverKind`) | Model | Effort |
| --- | --- | --- |
| `codex` | `gpt-6-astra` | `reasoningEffort: medium` |

**Role:** Have six Analysts look for simplifications of the subject diff and
turn their reports into validated proposals. Read-only: never edit files,
commit or publish; start no agents other than the Analysts below.

**Category tasks**

Give each category this task, with the questions an Analyst must answer for
this subject:

1. Smaller diff: is there a simpler or less invasive solution with the same
   behavior? Which edits are unnecessary, speculative, incidental churn or
   could touch fewer files and lines?
2. Redundant tests: which added tests, fixtures or scaffolding duplicate
   existing coverage, overlap each other or assert implementation details?
   Which can be removed or merged without losing meaningful coverage?
3. Reuse: which new code duplicates an existing helper, component, service,
   pattern, standard library or installed dependency? How would it be reused
   the way surrounding code uses it?

**Start six Analysts in parallel**

| Category | Codex Analyst | Claude Analyst |
| --- | --- | --- |
| 1 | `codex` `gpt-6.1-sol` `reasoningEffort: high` | `claudeAgent` `claude-opus-5-5` `effort: high` |
| 2 | `codex` `gpt-6-astra` `reasoningEffort: high` | `claudeAgent` `claude-fable-5-1` `effort: high` |
| 3 | `codex` `gpt-6.1-sol` `reasoningEffort: high` | `claudeAgent` `claude-opus-5-5` `effort: high` |

- Start all six at the same time, never one after another: do not wait for any
  Analyst before starting the rest. In T3 Code, send all six `delegate_task`
  calls in one response (`mode: "async"`, shared checkout, `role: "review"`)
  and follow `next` without polling.
- Give each the subject, the full context, its category task, user constraints
  and the absolute path of [analyst.md](analyst.md) to read first. Both Analysts
  of a category get the same brief. Do not share other Analysts' conclusions.
- Wait for all six to finish or become unavailable. Report unavailable Analysts
  without substituting models.

**Validate**

- Check each proposal against the code: it must preserve the intended behavior,
  public contracts and meaningful test coverage, and must not cut validation at
  trust boundaries, error handling that prevents data loss, security or
  accessibility.
- Drop speculative, unsubstantiated or out-of-scope proposals, redesigns and
  edits made only to produce a diff. Merge duplicates; resolve conflicting
  proposals by choosing the simpler one that preserves behavior.

**Report**

Return the validated proposals as a numbered list, most valuable first; the
main agent applies all of them, so include only proposals you would apply. For
each give its category, location, the concrete edit, why behavior stays the
same and the estimated diff reduction. "No worthwhile simplification" is a
valid answer. Below the list, add one line naming any unavailable Analyst.

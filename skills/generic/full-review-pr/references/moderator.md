# Moderator

| Provider (`driverKind`) | Model | Effort |
| --- | --- | --- |
| `codex` | `gpt-6-astra` | `reasoningEffort: medium` |

**Role:** Have eight Analysts review the PR and turn their reports into
validated findings. Read-only: never edit files, switch branches, commit or
publish; start no agents other than the Analysts below.

**Category tasks**

Give each category this task, with the questions an Analyst must answer for
this PR:

1. Critical bugs: which changes cause incorrect behavior, regressions, crashes,
   data loss, security holes or broken public contracts? Under which inputs or
   states do they fail?
2. Duplication: which new code repeats an existing helper, component, service,
   pattern, standard library feature or installed dependency, or repeats other
   code within the PR, without need? How would it reuse what exists?
3. Project rules: where does the PR contradict repository instructions,
   documented conventions or patterns the surrounding code consistently
   follows (structure, naming, error handling, testing, APIs)?
4. Jira requirements: which requirements and acceptance criteria of the
   supplied Jira task are missing, incomplete or implemented differently? What
   does the PR change that the task does not ask for? Skip this category and
   its two Analysts when no Jira task is supplied.

**Start the Analysts in parallel**

| Category | Codex Analyst | Claude Analyst |
| --- | --- | --- |
| 1 | `codex` `gpt-6-astra` `reasoningEffort: medium` | `claudeAgent` `claude-fable-5-1` `effort: medium` |
| 2 | `codex` `gpt-6.1-sol` `reasoningEffort: medium` | `claudeAgent` `claude-opus-5-5` `effort: medium` |
| 3 | `codex` `gpt-6.1-sol` `reasoningEffort: medium` | `claudeAgent` `claude-opus-5-5` `effort: medium` |
| 4 | `codex` `gpt-6-astra` `reasoningEffort: medium` | `claudeAgent` `claude-fable-5-1` `effort: medium` |

- Start all Analysts at the same time, never one after another: do not wait
  for any Analyst before starting the rest. In T3 Code, send all
  `delegate_task` calls in one response (`mode: "async"`, shared checkout,
  `role: "review"`) and follow `next` without polling.
- Title each Analyst `<model> - <category> Analyst` (`title` in
  `delegate_task`), with the model's family and version
  (`claude-opus-5-5` → `Opus 5.5`, `gpt-6.1-sol` → `Sol 6.1`), for example
  `Sol 6.1 - Performance Analyst`.
- Give each the PR, head SHA, base, the full context, its category task, user
  constraints and the absolute path of [analyst.md](analyst.md) to read first.
  Both Analysts of a category get the same brief. Do not share other Analysts'
  conclusions.
- Wait for all to finish or become unavailable. Report unavailable Analysts
  without substituting models.

**Validate**

- Check each finding against the code at the head SHA; drop speculative or
  unsubstantiated ones, style preferences the repository does not require and
  issues the PR did not introduce or worsen.
- Merge duplicates across Analysts and categories. Weigh agreement between the
  two Analysts of a category; investigate disagreements only when they decide
  whether a finding stands. Do not count votes.

**Report**

Return the validated findings as a numbered list, most severe first. For each
give its category, file and line, the problem, evidence, consequence and
suggested fix. "No findings" is a valid answer. Below the list, add one line
naming the categories covered and any unavailable Analyst.

# Quality Leader

| Provider (`driverKind`) | Model             | Effort           |
| ----------------------- | ----------------- | ---------------- |
| `claudeAgent`           | `claude-opus-5-5` | `effort: medium` |

**Role:** Check one snapshot of the Coder's work with six independent agents
and return validated findings to the Coder. Never edit the working tree,
commit, publish or fix findings; the Coder owns fixes. Do not invoke other
repository skills.

**Start six agents in parallel**

| Agents                   | Reference file                               | Count |
| ------------------------ | -------------------------------------------- | ----- |
| Code Reviewers           | [code-reviewer.md](code-reviewer.md)         | 2     |
| Critical Issue Reviewers | [critical-reviewer.md](critical-reviewer.md) | 2     |
| User Flow Testers        | [user-flow-tester.md](user-flow-tester.md)   | 2     |

- Use the provider, model and effort from each file. In T3 Code, start all six
  in one response via `delegate_task` (`mode: "async"`, shared checkout;
  `role: "review"` for reviewers, `role: "test"` for user flow testers) and
  follow `next` without polling.
- Title each agent `<model> - <role>` (`title` in `delegate_task`), with
  the model's family and version (`claude-opus-5-5` → `Opus 5.5`,
  `gpt-6.1-sol` → `Sol 6.1`), for example `Astra 6 - Code Reviewer`.
- Give each the same brief: the approved plan, the diff snapshot, repository
  instructions, the Coder's check results, the supplied URL, built artifact
  or command with setup instructions, and the absolute path of its own
  reference file to read first. Tell them they are read-only and must not run
  tests, lint, type checks or builds; the Coder runs them and supplies the
  results. Critical Issue Reviewers may reproduce a failure through the supplied
  access or a short script outside the checkout, and User Flow Testers exercise
  flows through the supplied access. The Coder manages services and prepares
  builds. Do not share other agents' conclusions.
- On a later round, pass on the previous findings, the Coder's fixes and the
  findings it declined, with reasons; agents check changes since then and
  whether those findings are resolved or rightly declined.
- Wait for all six to finish or become unavailable, even after a blocker
  appears. Report unavailable agents without substituting models.

**Validate**

- Check each finding against the code or the agent's evidence; drop
  speculative or unsubstantiated ones and merge duplicates.
- Investigate disagreements only when they decide a finding. Do not count
  votes or turn preferences into findings.

**Report to the Coder**

Return exactly one verdict:

- `changes needed`: at least one substantiated finding, whether or not there
  are also verification gaps. Report those gaps separately.
- `incomplete`: no substantiated findings, but an agent's report or evidence
  for a required user flow scenario is missing. A report saying a scenario
  could not be run does not count as verification of that scenario.
- `passes`: no substantiated findings, all six agents reported and every
  required user flow scenario was exercised with evidence and no gaps.

Include each agent with its model, the user flow scenarios and results with
evidence file paths, verification gaps, validated findings with category,
location, failure scenario and evidence, and dropped findings with reasons.

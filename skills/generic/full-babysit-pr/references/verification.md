# Verification

Verify the simplified local fix snapshot: the handled feedback, the CI failures
it addresses and affected behavior. Local setup and isolated probes are allowed;
production mutations are not.

**Core**

- Identify acceptance criteria, base and tested revision, including relevant local
  and untracked files. Follow repository instructions and run required checks.
- Split the criteria into claims and map each claim to at least one check that
  can disprove it. Choose the cheapest sufficient check: static checks, then
  related unit tests, then integration tests, then browser or e2e. Escalate only
  when a cheaper check cannot disprove the claim.
- Run independent checks in parallel; serialize prerequisites and checks that
  share mutable resources. Collect every result before reporting verification.
- Exercise affected behavior and meaningful failure cases. For content-only
  changes, check structure, consistency and links instead.
- When a failure looks unrelated to the change, rerun that check on the base
  revision; report failures that also occur there as pre-existing, separately
  from regressions.

**Optional, by context**

Run each step only when its context applies; skipping an inapplicable step is
not a verification gap.

- UI changes: inspect the changed scenario with the available browser MCP or host
  preview tools. Take screenshots and check layout at relevant viewports,
  interactions, loading, empty and error states, console errors and failed
  requests; compare with supplied designs or screenshots. Tests and typechecks
  alone cannot verify UI; if no browser tool is available, mark the UI claims
  `incomplete` and name the blocker.
- UI components: when the real app renders the component and that screen is
  quick to reach, verify it there only; do not repeat the check in docs,
  Storybook or other showcase pages. Use those pages only when no app screen
  uses the component, reaching it is impractical, or the user asks for them.
- E2E tests: run them only when the PR CI pipeline runs them or the user
  requests them.
- Bug fixes: when practical, show in an isolated checkout that the regression
  test fails on the base revision and passes on the tested one.
- Suspected flaky failures: rerun once; if results differ, report the check as
  flaky, and a claim supported only by it as `incomplete`.
- Mock mode: when the project can run with mocked APIs or services that need no
  login or live backend (for example MSW handlers, fixture servers or stub APIs),
  find how its scripts, configuration or documentation enable that mode and run
  behavior and UI checks there. Do not log in, use real credentials or call live
  or shared services for a claim the mocks can check. Label this evidence as
  mocked: it shows behavior against mocked responses, not the live integration.
- External dependencies: when no mock mode covers a claim, use local services,
  fixtures or practical substitutes, explaining their limits. Failed commands,
  unavailable dependencies and mocked integrations do not prove a claim about
  the live integration passes.

**Evidence**

- Retain commands, observed results and necessary visual evidence. Follow host
  browser rules, inspect artifacts before citing them and use an adequate
  alternative if capture fails.

Isolate probes, preserve user work and clean up only your resources. Diagnose
failures to an actionable cause; fixing them is the next maintenance round.

Record tested revision and local changes, environment, limitations and a table
mapping each claim to its checks, observed result and evidence path. Mark each
claim `passed`, `failed` or `incomplete`; list pre-existing failures, flaky
checks and skipped applicable optional steps separately. Success requires every
required claim to pass. Evidence applies only to tested code and environment;
rerun affected checks after changes and state the original scope of reused
evidence. Keep evidence local unless publishing it to the PR.

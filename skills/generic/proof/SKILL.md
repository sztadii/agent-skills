---
name: proof
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Proof

## Scope

Verify the requested claims on local code, a commit, a PR or a reproduction.
Local setup and isolated probes are allowed; implementation fixes, commits,
publication and production mutations require separate authorization.

## Dependencies

No skill dependencies

## Workflow

- Identify acceptance criteria, base and tested revision, including relevant local
  and untracked files. Follow repository instructions; run required checks and
  the smallest meaningful tests that can disprove the claims.
- Run independent checks in parallel; serialize prerequisites and checks that
  share mutable resources. Collect every result before reporting verification.
- Exercise affected behavior and meaningful failure cases. For content-only
  changes, check structure, consistency and links instead.
- For UI changes, inspect the changed scenario's appearance and interactions in
  a browser, including relevant loading, empty and error states. Tests and typechecks
  alone cannot verify UI; report incomplete UI claims and the blocker if browser
  verification is unavailable.
- Use local services, fixtures or practical substitutes as needed, explaining
  their limits. Failed commands, unavailable dependencies and mocked integrations
  do not prove the original claim passes.
- Retain commands, observed results and necessary visual evidence. Follow host
  browser rules, inspect artifacts before citing them and use an adequate
  alternative if capture fails.

Do not repair implementation to pass checks. Isolate probes, preserve user work
and clean up only your resources. Diagnose failures to an actionable cause.

## Result

Report tested revision and local changes, environment, checks, observed results,
evidence paths and limitations. Mark each claim `passed`, `failed` or `incomplete`;
overall success requires every required claim to pass.

Keep evidence local unless publication is authorized. It applies only to tested
code and environment; rerun affected checks after changes and state the original
scope of reused evidence.

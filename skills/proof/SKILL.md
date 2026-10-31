---
name: proof
description: Trigger this if the user says 'proof', or another skill invokes it.
disable-model-invocation: true
---

# Proof

Verify the actual changed behavior yourself in this session, following repo
setup and testing instructions. This skill does not authorize publishing,
production changes, or destructive actions.

1. **Identify what runs.** Confirm the checkout, revision, and local edits used
   by the running app or command, plus the environment and relevant data. Test
   the intended PR head when reviewing a PR; do not mistake a stale build or
   another server for the changed version.
2. **Control the scenario.** Prefer an existing mock server, sandbox, fixture,
   or equivalent controlled test environment when it faithfully exercises the
   change. Do not require or build one merely for this skill. State what is
   mocked; mock success does not verify an external integration. Exercise that
   integration when relevant and authorized, or report the remaining gap.
3. **Exercise the change.** For a bug fix, reproduce on the base and run the
   same scenario on the changed revision. For a feature, verify the requested
   behavior and relevant failure cases. Run the actual UI flow, endpoint, or
   command, plus the repo's required lint, typecheck, and tests as applicable.
   HTTP 200, source inspection, and green mocked tests alone are insufficient.
   Add a focused regression test when fixing a bug with missing coverage and
   an existing test setup; during review, report the gap without editing the
   author's code. For prose-only changes, use direct content/link validation
   instead of inventing runtime proof.
4. **Capture and inspect evidence.** For visual behavior, capture comparable
   before/after states with the same inputs, app state, viewport, and framing.
   Use recordings for interactions or timing and screenshots for static changes;
   if recording is unavailable, report the attempted capture method, concrete
   blocker, and what screenshots cannot prove. A passing E2E without a capture
   does not satisfy visual evidence, especially when it tests another scenario.
   For existing edits, obtain the base in an isolated environment without overwriting
   work; for new features, show the prior experience. Label revisions and keep
   artifacts local unless publication is authorized. Open both captures and play
   back recordings to verify readable, faithful evidence at a natural pace.
   When the calling workflow authorizes PR publication, pass the inspected
   artifacts to that workflow for attachment and access verification; a local
   capture alone does not complete the PR evidence step. For nonvisual behavior,
   retain reproduction commands and observed output.
5. **Verify the final state.** After edits, rebases, or conflict resolution,
   rerun affected checks and scenarios; earlier proof does not cover changed
   behavior. Report the verified revision/local state, scenario, expected versus
   observed result, check outcomes, evidence links, and remaining gaps.

If a baseline, capture, integration, or check cannot be obtained, say what you
tried and what remains unverified. Continue useful checks, but never invent
missing evidence or claim complete proof or 100% correctness from partial results.

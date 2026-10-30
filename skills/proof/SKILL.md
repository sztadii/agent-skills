---
name: proof
description: Use when the user asks for proof or another skill requires it.
---

# Proof

Verify the actual changed behavior locally. Follow the repository's setup and
testing instructions and the invoking workflow's permissions; this skill does
not itself authorize publishing, production changes, or destructive actions.

Proof is evidence you observed yourself in this session: command output, a
reproduction, a recording, or a screenshot. Use the most direct proof the
change allows:

- For a bug fix, reproduce the failure on the base revision, then verify the
  same reproduction passes on the changed revision.
- Run the checks that the repo's CI runs (typecheck, lint, tests).
- When implementing a fix, add one focused regression test if coverage is
  missing and the repo has a test setup. During review, report missing coverage
  rather than modifying the author's code; temporary local probes are fine.
- For UI and runtime behavior, run the app and look at the change. Green mocked
  tests do not prove that the user experience works. Prefer recordings over
  screenshots when visual evidence helps. Keep artifacts local unless posting
  them to the PR is authorized.
  Use a clear, readable resolution; 1080p is sufficient. Keep recordings short
  and focused on the relevant flow, omitting setup and unnecessary idle time
  without speeding up the interactions.
  Demonstrate the flow at a natural manual pace: let each action's visible
  result settle before the next, leaving time to read feedback. Avoid rapid
  repeated actions that create duplicate toasts or other distracting artifacts.
  Use screenshots when recording is unavailable or a still image better shows
  the result.

If verification is blocked, report what you tried, what is missing, and which
behavior remains unverified. Ask for the missing input when necessary; do not
claim full confidence from source inspection or green CI alone.

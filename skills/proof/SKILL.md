---
name: proof
description: Use when the user asks for proof or another skill requires it.
---

# Proof

Verify the actual changed behavior locally. Follow the repository's setup and
testing instructions and the invoking workflow's permissions; this skill does
not itself authorize publishing, production changes, or destructive actions.

Before claiming the change works, confirm that the running app uses the intended
checkout and commit (the current remote head when testing a branch or PR), then
exercise the specific behavior being fixed. An HTTP 200 response alone does not
prove that the app or the fix works.

Proof is evidence you observed yourself in this session. For UI and other
visually observable behavior, screenshots or recordings of both the before and
after states are required; command output and tests supplement that evidence.
Use the most direct proof the change allows:

- For a bug fix, reproduce the failure on the base revision, then verify the
  same reproduction passes on the changed revision.
- Run the checks that the repo's CI runs (typecheck, lint, tests).
- When implementing a fix, add one focused regression test if coverage is
  missing and the repo has a test setup. During review, report missing coverage
  rather than modifying the author's code; temporary local probes are fine.
- For UI and runtime behavior, run the app and exercise the affected flow.
  Capture the before evidence before editing, then capture the same flow after
  the change using comparable inputs, app state, viewport, and framing. During
  review or when edits already exist, capture the base revision in a separate
  checkout or equivalent isolated environment without overwriting current work.
  For a new feature, capture the existing experience where the feature will
  appear. Label the artifacts clearly as before and after and identify the
  revision used for each.
  Green mocked tests do not prove the user experience works. Use recordings for
  interactions, transitions, and timing, and screenshots for static changes or
  when recording is unavailable. Report any resulting verification gap. Keep
  artifacts local unless posting them to the PR is authorized.
  Prefer native 1080p captures (landscape or portrait); do not upscale. Record at
  a stable 30 FPS minimum, preferably 60 FPS for motion when supported. Play back
  the exported recording to verify readable details, clean encoding, and smooth
  motion. Re-capture capture defects; preserve and report app defects. If capture
  tools cannot meet these targets, report the actual resolution/FPS and limitation.
  Keep recordings short and focused on the relevant flow, omitting setup and
  unnecessary idle time without speeding up the interactions.
  Demonstrate the flow at a natural manual pace: let each action's visible
  result settle before the next, leaving time to read feedback. Avoid rapid
  repeated actions that create duplicate toasts or other distracting artifacts.

Inspect both captures and compare the observed outcome against the requested
behavior. In the final report, link both artifacts and explain what they show.
For behavior with no meaningful visual output, use a comparable before/after
reproduction and its command output instead; do not create decorative captures.
If either visual capture cannot be obtained, explicitly report the missing
evidence and the reason. Do not present a complete before/after proof or invent
the missing state.

If verification is blocked, report what you tried, what is missing, and which
behavior remains unverified. Ask for the missing input when necessary; do not
claim full confidence from source inspection or green CI alone.

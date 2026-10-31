---
name: proof
description: Trigger this if the user says 'proof', or another skill invokes it.
disable-model-invocation: true
---

# Proof

Verify the changed behavior locally and retain evidence suited to the change.
Authorizes ordinary setup and execution, not publication, production changes
or destructive actions.

- Confirm the revision, local diff and relevant environment. Exercise the changed
  UI, endpoint or CLI and relevant failure cases. For content-only changes,
  validate the content and links instead.
- Run repository-required checks and affected verification. If a live dependency
  is unavailable, use a simulator, fixture or minimal mock when practical;
  identify what it cannot verify.
- Choose evidence that clearly demonstrates the result: screenshots for visible
  states, a recording when motion or a sequence matters, or commands and observed
  output for nonvisual behavior. Before/after evidence is optional.
- Inspect screenshots and other artifacts; play recordings before using them.
  If recording is unavailable or adds little value, use screenshots or other
  suitable evidence. A missing recording alone is not a verification gap and
  does not require an exception. Do not keep retrying capture when an adequate
  alternative exists.
- Report what was exercised, the observed result, checks and artifact paths,
  and any behavior that remains unverified. Evidence must support the claim:
  a screenshot of the final state alone does not prove timing or a whole flow,
  and mock success does not prove a live integration.

After behavior edits or conflict fixes, rerun affected verification. Read-only
reviews do not edit author code. Keep artifacts local unless the calling workflow
authorizes publication; pass inspected evidence to file-pr. If blocked, state
what remains unverified without claiming complete proof.

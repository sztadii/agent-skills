---
name: kineti-full-send
description: "Trigger only when the user or another skill explicitly invokes this skill for Kineti delivery with MSW-only verification and a PR recording."
---

# Kineti Full Send

## Scope

Invoke [$full-send](../../../generic/full-send/SKILL.md) with these overrides;
pass them to `$proof`, `$file-pr` and `$babysit-pr`, including after repairs.

## Dependencies

Required: [$full-send](../../../generic/full-send/SKILL.md).

Before starting, find each required skill in the agent's catalog or installed
locations, read its SKILL.md and complete its dependency check. Repository links
alone are insufficient. If any skill is missing or unreadable, stop this workflow
and its caller; name the affected skills without substituting or installing.

## Workflow

- During implementation, update MSW handlers and fixtures for changed behavior
  and acceptance scenarios before running `$proof`.
- Exercise behavior only against MSW. Verify relevant requests are mocked; prevent
  unhandled API requests from reaching real services. Fix missing coverage before
  claiming `$proof` passed. Keep required repository checks; label evidence as mocked
  behavior, not live backend integration.
- Record and inspect the working acceptance scenario with MSW enabled. Through
  `$file-pr`, publish the recording or accessible link in the PR description; refresh
  it after behavior changes. Capture/publication failure leaves delivery incomplete;
  screenshots cannot replace the recording.

## Result

Return the result of `$full-send` with MSW verification evidence and the PR
recording or publication link. Missing evidence or recording means incomplete.

---
name: find-improvements
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Find Improvements

## Scope

Analyze the current repository unless another repository or narrower focus is
selected. Start without parameter questions; report unavailable or ambiguous
repositories rather than substituting.

Propose two bugs and two refactorings for the user's decision. Work read-only:
do not edit files, install dependencies, switch checkout, implement fixes, create
commits/PRs/tickets or change schedules.

## Dependencies

No skill dependencies

## Workflow

**Analysis**

- Record start time, repository, branch and HEAD SHA. Read repository instructions,
  relevant architecture and tests. Preserve local changes; distinguish them from
  committed code.
- Start with recent commits/diffs, prioritizing changes since a previous report's
  SHA if it is an ancestor of HEAD. Otherwise state the recent range reviewed.
  Continue into related code even without new commits.
- Spawn exactly two subagents: one searches for up to two bugs, the other for up
  to two refactorings. Give both the scope, baseline, read-only constraints,
  candidate criteria and analysis deadline. Do not spawn one agent per finding
  or let these agents delegate further. If delegation is unavailable, report
  the limitation rather than silently replacing it with solo analysis.
- Actively search and verify for at least ten elapsed minutes; parallel work
  does not shorten this requirement. Challenge candidates even if four appear
  early; never fill time with waits. Trace callers, contracts, state and data
  flow; prioritize regressions before broadening. Attribute findings to commits
  only with evidence.
- Check accessible open PRs, previous reports and user decisions for duplicates,
  work in progress and rejected ideas; disclose unavailable sources. Validate
  subagent evidence, reject weak or duplicate findings and rank each category by
  value relative to effort. If blocked, report actual duration and cause.

**Candidates**

**Bugs:** concrete, reachable behavioral errors requiring an estimated 30–60 changed
implementation lines each, excluding tests. Derive trigger, expected behavior and
consequence from code and contracts. Exclude hypothetical risks, style preferences
and unsupported product assumptions. Use existing tests or non-mutating
reproductions where useful; distinguish execution from code analysis.

**Refactorings:** behavior-preserving improvements within roughly 200 changed
implementation lines each, excluding tests. Explain the simplification,
deduplication or structural change and concrete maintenance benefit. Exclude features,
cosmetic changes and speculative abstractions.

Increase limits only with user authorization for that category. Estimate honestly;
report fewer proposals if evidence or scope cannot support two. Do not invent findings.

## Result

Respond here with repository, branch, HEAD SHA, reviewed range, elapsed time and
coverage/verification limits. Retain the SHA for later runs without state files.

Use **Bugs** (B1/B2) and **Refactors** (R1/R2). For each proposal include:

- Trigger and consequence, or proposed refactoring and benefit.
- File links with line numbers, code-based justification, related commit if
  established, verification and a useful reproduction or regression check.
- Estimated implementation lines excluding tests, and behavior to preserve.

State shortages. End with proposals for user selection; do not implement or ask
for approval per item.

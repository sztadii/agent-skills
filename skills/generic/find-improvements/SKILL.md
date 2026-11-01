---
name: find-improvements
description: "Analyze repository code and propose two small bugs and two worthwhile refactorings for the user's decision. Trigger only on explicit invocation by the user, a scheduled task or another skill."
---

# Find Improvements

Run only when explicitly invoked as `$find-improvements` (Codex),
`/find-improvements` (Claude Code), or by another skill. An explicit invocation
in a scheduled task runs the same workflow without interactive setup. Mentions
and matching tasks alone do not trigger it.

The invocation alone analyzes the current repository. Optional context can
select another repository or narrow the focus. Start with these defaults without
asking for parameters. If the selected repository is unavailable or ambiguous,
report the missing context rather than analyzing an unrelated repository.

Prepare proposals for the user's decision: two small behavioral bugs
and two refactorings. This is a read-only analysis: do not edit repository files,
change checkout, implement fixes, create commits, PRs or tickets, or change
schedules. Implementation is a separate task selected by the user. Do not start
subagents.

## Analysis

1. Record the start time, repository, branch and HEAD SHA. Read repository
   instructions and enough architecture, entry points and test configuration to
   understand the relevant behavior. Preserve existing local changes; distinguish
   uncommitted code from committed changes in the findings.
2. Always begin with the latest commits and their diffs. If an accessible previous
   report provides a reviewed SHA that is an ancestor of HEAD, inspect changes
   since that SHA first. Otherwise inspect recent commits and state the range
   covered. Do not reset or switch the checkout to obtain a baseline. Even with
   no new commits, continue into related code rather than assuming no findings.
3. Trace changed code through callers, dependencies, state transitions, types,
   data flow and comparable implementations. Prioritize regressions and
   inconsistencies with established behavior, then broaden to related and more
   distant areas. Attribute a finding to a recent commit only with evidence.
4. Check accessible open PRs and their actual scope to avoid proposing work
   already in progress. Consult accessible previous reports and user decisions
   to avoid repeating unchanged proposals or rejected ideas. If these sources
   are unavailable, disclose that limitation and continue local analysis; do not
   invent history or claim deduplication was complete.
5. Spend a full ten minutes actively searching and verifying. Check elapsed time
   periodically. Finding four candidates early does not end the analysis: trace
   more paths, challenge assumptions, reject false positives and seek stronger
   candidates. Do not fill time with idle waits. After ten minutes, select the
   strongest verified proposals and write the report. If blocked from completing
   the analysis, report the actual duration and specific blocker.

## Candidate criteria

**Bugs:** choose two concrete, existing behavioral errors, each with an estimated
30–60 lines of implementation changes, excluding tests. Establish a reachable
trigger, expected behavior and actual consequence by tracing the code and its
contract; cite precise locations. A hypothetical risk, style preference or
unsupported product assumption is not a confirmed bug. Use existing tests or a
small non-mutating reproduction when useful and available; distinguish code
analysis from executed verification. Do not install dependencies or alter files
solely to reproduce a candidate; report any resulting verification limit.

**Refactorings:** choose two improvements to existing code, each within roughly
200 changed implementation lines, excluding tests. Preserve product behavior.
Prefer justified simplification, removal of duplication or clearer structure
with a concrete maintenance benefit. Exclude new features, purely cosmetic
changes and abstractions without demonstrated value. Explain what would change
and which existing behavior must stay intact.

Estimate the actual scope without inflating or understating it to fit these
targets. Aim for two findings in each category, but report fewer if evidence or
scope does not support the target; say what is missing instead of manufacturing
findings. Keep the categories distinct and rank each by value relative to effort.

## Report

Respond in the current conversation. Start with a short scope line:
repository, branch, reviewed HEAD SHA, commit range, elapsed analysis time and
material coverage or verification limitations. Include the SHA so a later run
can reuse it without writing state files. Do not imply complete repository
coverage when only selected paths were reviewed.

Use sections **Bugi** and **Refaktoryzacje**, with concise numbered proposals
identified as B1/B2 and R1/R2. Each proposal should include:

- One sentence describing the trigger and consequence (bug), or the proposed
  change and concrete benefit (refactoring).
- File links with line numbers and a short code-based justification; include the
  related commit when established. State how the bug was verified and give a
  compact reproduction or regression-check idea when useful.
- Estimated implementation lines to change, excluding tests. For a refactoring,
  mention the behavior that verification should preserve.

State shortages explicitly, including when there are no credible findings. End
with the proposals; leave selection to the user without requesting approval for
each item or beginning implementation.

---
name: council
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Council

## Scope

Independently review a diff, commit range, PR or proposal using evidence. Do not
change the supplied version, publish, commit, push or start delivery; the caller
owns fixes and preparation.

## Dependencies

No skill dependencies

## Workflow

**Prepare**

- Establish intent, constraints, actual base, head and relevant local/untracked
  changes; for proposals, record the version and assumptions.
- Record the snapshot and evidence. Give all reviewers the same stable snapshot;
  if the target changes during review, identify the reviewed version and report
  the unreviewed changes as a coverage gap. Isolate executable probes and preserve
  the author's checkout and remote.

**Review**

Use the user's reviewers and settings; otherwise use two independent agents with
inherited settings on the current provider and complementary focuses. Follow host
delegation rules. Give each the same intent, snapshot, repository instructions and
evidence, without other reviewers' conclusions.

Start reviewers in parallel; collect all reviews before synthesizing findings.
Do not finish early because one reviewer found a blocker; wait for every reviewer
to finish or become unavailable.

Reviewers assess correctness, integration, complexity, diff size and test value.
Request severity, location, concrete failure scenario, evidence, assumptions and
verification limits. Allow isolated read-only probes, but no edits, publication,
PR watching or further delegation.

Validate and deduplicate findings; investigate disagreements. Separate demonstrated
defects, material risks and optional preferences. Do not count votes or require
cosmetic edits for unanimity.

Report unavailable reviewers without substituting providers or models; complete
available reviews. Coverage is incomplete without the requested lineup and count
(two by default); your review cannot replace an independent one. Ask only if
continuing requires changing the requested configuration.

## Result

Return the snapshot, verification, actual reviewers, findings, material
disagreements and a verdict explained with review evidence:

- `clear`: adequate coverage and no substantiated blockers; explain why none remain
  in scope. This is not a correctness guarantee.
- `changes needed`: identify substantiated blockers and recommended corrections,
  even when coverage is incomplete; report missing evidence or reviewers separately.
- `incomplete`: no substantiated blockers found, but evidence or coverage is
  insufficient for `clear`; identify the gaps.

Identify included local changes to distinguish the snapshot from the published
head. Return comments to the caller without publishing or fixing code; do not
invent scores or approvals.

End the invocation with this report. The caller owns fixes and any new review;
do not wait for corrections, start further rounds or carry forward earlier verdicts.

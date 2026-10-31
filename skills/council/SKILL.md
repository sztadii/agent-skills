---
name: council
description: Obtain independent agent reviews of a change, design or implementation decision and reconcile their findings. Use for a second opinion without editing or publishing the reviewed work.
disable-model-invocation: true
---

# Council

Return an evidence-backed decision from independent reviewers. Accept a local
diff, commit range, PR or concrete proposal. No PR or prior proof run is required.
Council and its reviewers do not edit the subject, publish comments, commit,
push or initiate delivery. The caller owns any subsequent fixes.

## Independent review

1. Establish intent, constraints and the exact subject. For code, record the actual
   base, head and local changes, including relevant untracked files. For a proposal,
   identify its version and assumptions. Keep that snapshot stable during review;
   do not silently review a moving branch.
2. Honor explicitly selected reviewers, providers, models and settings. Otherwise
   use two independent agents on the current provider with inherited settings;
   assign complementary focuses such as correctness/integration and
   maintainability/tests. A specific provider mix is not a default requirement.
   Follow the host's delegation rules. In T3 Code, discover capabilities before
   the first delegate_task; use native agents when the host permits them.
3. Give each reviewer the same intent, snapshot, relevant repository instructions
   and available evidence, without another reviewer's conclusions. Ask for
   actionable findings with severity, location, concrete failure scenario and
   evidence, plus assumptions and verification limits. Reviewers may run isolated
   read-only probes but must not edit, publish, watch PRs or delegate further.
4. Check each finding against the actual subject, deduplicate overlapping reports
   and investigate disagreements. Distinguish demonstrated defects, material
   risks and optional preferences. Do not count votes or require cosmetic changes
   to manufacture unanimous approval.

If a requested reviewer is unavailable, report it without substituting another
provider or model. Complete available reviews. Assess completeness against the
user's requested lineup and count, or two independent reviewers by default. An
explicit one-reviewer request can complete with that reviewer; two of three
required reviewers is incomplete. Your own review is supplementary, not a missing
independent vote. Unavailability needs user input only if changing the requested
reviewer configuration is necessary to continue.

## Result

Return the reviewed snapshot, actual reviewers, consolidated findings, material
disagreements and a verdict: `clear`, `changes needed` or `incomplete`. Clear means
no substantiated blocking finding and adequate review coverage, not a guarantee
of correctness. Do not invent scores or approvals.

For a re-review, start a new review round with the changed snapshot and previous
unresolved findings. Retain earlier conclusions as history, not approval of new
code. Investigate new evidence; stop repeating a round that yields no new evidence
or progress and identify the concrete unresolved issue.

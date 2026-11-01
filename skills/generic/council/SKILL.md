---
name: council
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Council

Run only when the user explicitly invokes `$council` (Codex) or
`/council` (Claude Code), or another skill explicitly instructs you to invoke
this skill.
Mentions, examples and matching tasks alone do not trigger this workflow.

## Required skill check — first action

Required skills: [$simplify](../simplify/SKILL.md).

At entry, check once through the host's skill discovery that the skills listed
above are installed and their `SKILL.md` files are readable. If any are unavailable,
stop the entire workflow, including the caller, and tell the user which are
missing. Do not skip, substitute or auto-install them. After this check succeeds,
proceed with the workflow below without repeating the availability check.

## Purpose and scope

Return an evidence-backed decision from independent reviewers. Accept a local
diff, commit range, PR or concrete proposal. No PR or prior proof run is required.
For code, council owns a simplify pass before independent review. Reviewers never
edit the subject. Council does not publish comments, commit, push or initiate
delivery; the caller owns fixes prompted by review findings.

## Prepare code before review

1. Establish intent, constraints, actual base, original head and local changes,
   including relevant untracked files. For a design or proposal without code,
   record its version and assumptions and go directly to independent review.
2. Invoke [$simplify](../simplify/SKILL.md) once for the whole code review round,
   before starting any reviewer.
   For a local task, apply it to that task's diff, preserving unrelated work. For
   a PR or commit range, prepare an isolated local working copy of the exact
   subject and simplify there; preserve the author's checkout and remote branch.
   If preparation cannot respect the user's scope, stop and report the blocker.
   Simplify may return a justified no-op; do not force edits or loop until the
   diff reaches an arbitrary size. If its verification is incomplete or failed,
   return incomplete with the gap rather than starting reviewers.
3. Record the prepared head plus local diff, simplify's edits or no-op, and its
   verification evidence. Earlier evidence applies only where still valid after
   simplification. Freeze this prepared snapshot for all reviewers. Do not run
   simplify separately per reviewer or edit the subject during their review.

## Independent review

1. Review the prepared code snapshot or the identified proposal. All reviewers
   must assess the same subject; do not silently review a moving branch. Earlier
   simplification does not establish readability: reviewers still assess needless
   complexity, diff size and test value as well as correctness.
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

Return the original and reviewed snapshots, simplification changes or no-op and
verification evidence, actual reviewers, consolidated findings, material
disagreements and a verdict: `clear`, `changes needed` or `incomplete`. Clear means
no substantiated blocking finding and adequate review coverage, not a guarantee
of correctness. Do not invent scores or approvals. If simplification changed a PR
or commit range in an isolated copy, return the preparation diff and make clear
that the verdict covers that local variant, not the unchanged published head.

For a re-review, start a new review round with the changed snapshot and previous
unresolved findings. Run simplify once before reviewers in each new code review
round; do not repeat it after findings within the same round. Retain earlier
conclusions as history, not approval of new code. Investigate new evidence;
stop repeating a round that yields no new evidence
or progress and identify the concrete unresolved issue.

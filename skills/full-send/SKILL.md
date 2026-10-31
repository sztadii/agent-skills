---
name: full-send
description: Deliver a feature or fix autonomously from initial context through implementation, verification, independent review and PR maintenance. Use when the user wants the whole delivery process owned end to end.
disable-model-invocation: true
---

# Full Send

Own delivery from the initial request to the requested endpoint. Invocation
includes implementation, local setup and verification, independent agent review,
task commits and pushes, PR publication, ready-for-review status, feedback replies
and in-scope CI/conflict fixes. Default delivery is a ready PR with current-head
required checks and reviews satisfied and actionable feedback settled. If the
initial request includes merging or deployment, continue through that endpoint
under repository policy; never infer production deployment from “full-send”.

Honor narrower instructions such as “draft only” or “do not push”. Do not ask for
routine phase approvals, approval of a prepared PR, selection of individual
comments or permission already given. Make reversible implementation choices
from the request and repository conventions. Escalate only an unresolved product
choice, missing access or action outside the authorized scope that actually blocks
progress; complete independent work first and explain the exact blocker.

## Deliver

1. **Establish the contract.** Read initial context and repository instructions,
   inspect existing work and infer acceptance criteria, actual base and destination.
   Record scope, completion condition and available verification. Reuse a matching
   task branch/PR and preserve unrelated changes. Do not require a ticket, update
   an issue tracker or ask the user to supply facts available in the repository.
2. **Implement.** Make the complete in-scope change, including meaningful regression
   coverage and required documentation. Resolve integration issues, inspect the
   diff and simplify worthwhile complexity while preserving behavior. Do not turn
   cleanup into a separate redesign.
3. **Verify.** Run required repository checks and exercise the acceptance criteria.
   Inspect evidence suited to the behavior. Diagnose failures and fix their causes;
   a mock or screenshot proves only what it exercises. A missing optional artifact
   is not a blocker if adequate evidence exists.
4. **Review and repair.** Obtain two independent reviews with inherited settings
   by default, following host delegation rules and any user-selected reviewers.
   Keep the reviewed snapshot stable and forbid reviewer edits or further
   delegation. Validate findings, fix substantiated in-scope issues and refresh
   affected checks and review after substantive edits. Cosmetic changes do not
   restart the whole process. Stop repeating unsuccessful rounds when no new
   evidence or progress is possible; report the remaining issue honestly.
5. **Publish.** Commit only task changes, push and create or update the matching PR
   with a concise problem/result description, current verification and useful
   evidence. Follow the host's immediate PR-linking rules. A complete verified
   change is ready for review unless draft status was requested. If required
   verification or independent review is unavailable, preserve useful work as a
   clearly labelled draft; report the delivery as incomplete, never silently
   waive the gap. Inspect remote state before retrying ambiguous writes.
6. **Maintain to the endpoint.** Handle current-head CI failures, valid review
   feedback and necessary base updates/conflicts. Verify fixes before pushing,
   reply with evidence and refresh invalidated review. Do not rewrite a shared
   branch without authority; an authorized rewrite uses a lease. Preserve draft
   status if requested; otherwise mark a workflow-owned draft ready once its gaps
   are closed. Wait for actual required checks/reviews using the host's event
   watcher, not polling. Resume on events until the completion condition is met.
   If merging was requested, respect protection and verify the expected head and
   actual merge. If deployment was requested, verify the specified environment.

## Composition and continuation

When installed, these skills can handle their respective phases; load their
instructions only when used. They are helpers, not prerequisites, and never need
full-send to run themselves:

- [simplify](../simplify/SKILL.md): focused cleanup and affected validation.
- [proof](../proof/SKILL.md): behavioral checks and evidence.
- [council](../council/SKILL.md): independent review and reconciliation.
- [file-pr](../file-pr/SKILL.md): commit, push and PR publication.
- [babysit-pr](../babysit-pr/SKILL.md): maintenance, feedback and event waiting.
- [leaving-pr-comments](../leaving-pr-comments/SKILL.md): requested communication.

If a helper is absent, perform its phase using the workflow above. Pass scope,
authorization, exact revision, evidence and endpoint to helpers; a helper's
completion is not completion of the whole delivery. Full-review is a separate
review workflow, not a mandatory phase of feature delivery.

Keep a compact continuation record in the host's task state: acceptance criteria,
branch/base/head, local diff, PR URL, checks/evidence, review findings, blockers and
next action. In T3 Code the parent arms the PR watcher and ends the turn while
waiting; delegated reviewers never watch. Remove the watch when finished, stopped
or handing back a blocker. Do not promise continued monitoring without an active
watcher. If monitoring is unavailable, report pending work as incomplete.

Report what was delivered, evidence, PR URL and actual ready/merged/deployed state.
An open draft, pending required checks or missing approval is not a completed
default delivery. External blockers may need intervention; do not manufacture
success, bypass required approval or repeatedly ask the same question.

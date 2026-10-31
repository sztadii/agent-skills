# My global skills

Install all skills globally using the [skills package](https://github.com/vercel-labs/skills):

```sh
npx skills add sztadii/agent-skills -g --skill '*'
```

To install just one skill, for example `full-review`, run:

```sh
npx skills add sztadii/agent-skills -g --skill full-review
```

## Usage examples

### 1. Fix a small bug and verify it — `proof`

> Fix the CLI crash when the output directory does not exist. Create the directory automatically and use proof to verify the fix with a real command invocation.

Reproduces the failure, applies a focused fix, and runs the affected command
locally. Reports observed results and adds regression coverage when needed.

### 2. Implement a Jira ticket with a pre-PR review — `full-send`

> Implement APP-142: prevent duplicate form submissions. Full send.

Assigns the linked ticket to your verified Jira account and moves it to the
workflow's development status. Implements, verifies, and independently reviews
the change, then shows you the diff, evidence, and proposed PR text. Waits for
your approval before creating a draft. Marking it ready requires your explicit
request; Jira moves to In Review when the work is actually handed off for review.

### 3. Finish an existing PR and update Jira after merge — `full-send`

> PR #84 implements APP-142 and is already ready for review. Full send: address remaining feedback, verify the changes, and merge once the merge gate passes.

Verifies and reviews the change, handles CI and feedback, then merges when
the gate passes and moves the linked Jira issue to the workflow's equivalent
of Ready for QA / To Test. No duplicate PR or creation approval is needed.
Use `jira-status` separately when you want milestone tracking without full send.

### 4. Open a draft PR for completed work — `file-pr`

> Use file-pr to open a draft PR for the pagination fix on this branch. Explain that page 2 previously repeated page 1, and include the verification results.

Checks for an existing PR, reviews the diff against the base, and writes a
concise title and description following repository conventions. Creates a
draft PR without marking it ready or merging it.

### 5. Review a teammate's PR — `full-review`

> Full review of PR #91. Run the affected export flow locally and report bugs and worthwhile simplifications here.

Reviews an isolated checkout and exercises the changed behavior. Returns
findings with evidence without modifying the author's branch or publishing
comments.

### 6. Compare reviewers' views on a sensitive change — `council`

> Council review of the session renewal changes on this branch. Focus on token expiry, concurrent requests, and logout behavior. Keep fixes within this change.

Gets independent reviews from three providers, compares findings, and applies
justified fixes. Requests fresh review after fixes and reports scores and
remaining disagreements.

### 7. Simplify an overbuilt implementation — `simplify`

> Simplify the retry logic in my current diff. Preserve the retry limit, backoff behavior, and existing error handling.

Reviews code structure, tests, and repository conventions with parallel agents.
The main agent evaluates their proposals and applies worthwhile simplifications,
runs relevant checks, and reports diff size before and after. Does not commit,
push, or open a PR on its own.

### 8. Keep an open PR moving through CI and review — `babysit-pr`

> Babysit PR #84. Fix CI failures and valid review findings, verify each fix, and leave the PR open for me.

Handles current failures and feedback, then uses the supported PR watcher for
new events. Preserves draft status and reports readiness or blockers without
merging under this request.

### 9. Deliver a task for your inspection — `full-send`

> Add a Clear filters button to the orders table. It should reset all filters and return to page 1. Full send.

Implements the task, verifies actual behavior, and obtains independent review.
Shows you the complete diff, evidence, and proposed PR title and description
before asking for approval to create a draft. After you request ready status,
handles CI and review and merges only when the merge gate passes.

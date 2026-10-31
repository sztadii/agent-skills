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

### 2. Implement a Jira ticket and track its status — `jira-status` + `full-stop`

> Implement APP-142: prevent duplicate form submissions. Use $jira-status and full stop. Open a PR and leave it unmerged.

Reads the ticket, assigns it to you, and moves it to the project's equivalent
of In Progress. Implements, verifies, reviews, and opens a draft PR. Jira moves
to In Review only when the work is actually awaiting review; creating a draft
alone does not establish that milestone. The PR stays unmerged.

### 3. Finish an existing PR and update Jira after merge — `jira-status` + `full-send`

> PR #84 implements APP-142 and is already ready for review. Use $jira-status and full send: address remaining feedback, verify the changes, and merge once the merge gate passes.

Verifies and reviews the change, handles CI and feedback, then merges when
the gate passes and moves Jira to the project's equivalent of Ready for QA /
To Test.

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

### 6. Get independent feedback before opening a PR — `sol-loop`

> Run sol loop on my CSV export changes against main. Focus on escaping, empty results, and compatibility with existing callers.

Uses one GPT Sol reviewer, evaluates findings, and makes minimal justified
fixes. Defaults to at most three review rounds, with later rounds focused on
changed code and unresolved findings. Reports any remaining disagreements.

### 7. Compare reviewers' views on a sensitive change — `council`

> Council review of the session renewal changes on this branch. Focus on token expiry, concurrent requests, and logout behavior. Keep fixes within this change.

Gets independent reviews from three providers, compares findings, and applies
justified fixes. Requests fresh review after fixes and reports scores and
remaining disagreements.

### 8. Simplify an overbuilt implementation — `simplify`

> Simplify the retry logic in my current diff. Preserve the retry limit, backoff behavior, and existing error handling.

Reviews the whole change and nearby conventions with a Sol reviewer. Removes
unnecessary complexity where useful, runs relevant checks, and reports the
diff size before and after. Does not commit, push, or open a PR on its own.

### 9. Keep an open PR moving through CI and review — `babysit-pr`

> Babysit PR #84. Fix CI failures and valid review findings, verify each fix, and leave the PR open for me.

Handles current failures and feedback, then uses the supported PR watcher for
new events. Preserves draft status and reports readiness or blockers without
merging under this request.

### 10. Deliver a small task as a PR — `full-stop`

> Add a Clear filters button to the orders table. It should reset all filters and return to page 1. Full stop.

Implements the task, verifies the actual behavior, runs Sol review, opens a
draft PR, and handles available CI and review feedback. Hands off the draft
PR with proof and any blockers.

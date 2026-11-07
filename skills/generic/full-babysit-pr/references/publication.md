# Publication

Publish user-approved fixes to the existing PR. Do not implement features,
perform broad cleanup, change draft/ready status or merge here.

**Prepare**

- Identify the PR's head branch, base and remote. Inspect staged, unstaged and
  untracked changes and outgoing commits; commit no unrelated work. Publish only
  to the PR's head branch.
- Inspect the full diff. Reuse verification of identical content or rerun affected
  checks and repository-required validation. Stale reviews do not approve new
  changes.
- For ready PRs, report blockers without pushing unverified changes. Drafts may
  disclose failed required checks or unverified behavior in the PR text. If
  publication requires passing verification, stop before remote changes; do not
  ask the user to waive routine verification.

**Commit and push**

- Follow repository commit conventions; if none are documented, infer title
  style, language, scope and body from recent commits.
- Never bypass hooks (`--no-verify`, commit `-n`, `HUSKY=0` or equivalents).
  Fix failures within the approved scope or stop and report.
- Inspect committed output and hook changes. Commit required generated files
  exactly as produced: ordering matters even when behavior is unchanged.
- Run required regeneration and checks on the final revision until generation
  leaves no diff. Hooks alone are insufficient. Results for identical content
  satisfy this; rerun after any change, including hook edits. If hook fixes change
  approved behavior, report them to the user before pushing.
- Recheck the remote head before pushing; never overwrite remote commits without
  authorization and a lease tied to that head. Confirm the pushed revision.
  Inspect remote state before retrying ambiguous failures.

**Update the PR**

- Update the PR text only where the fixes change described behavior, checks or
  limitations. Follow repository templates and conventions; preserve unrelated
  user-authored content. Describe actual checks and inspected evidence; do not
  copy other PRs' claims or approvals. Check text and attachments for private data.
- Preserve existing draft/ready status, including manual changes; never promote,
  demote or merge here.
- In T3 Code, link the PR with `link_pull_request` and its full URL after each
  push. Verify URL, base, remote head, body and status. Inspect remote state
  before retrying ambiguous API failures. Use REST if GraphQL is unavailable.

Record commits, published head, verified status, checks, gaps and remaining
local changes; report pending CI as pending.

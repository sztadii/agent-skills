---
name: full-create-pr
description: "Commit and push prepared changes with Git hooks and open a draft PR, attaching supplied screenshots or recordings when available. Trigger only when the user explicitly invokes this skill."
---

# Full Create PR

## Scope

Publish prepared working-tree changes as a draft pull request, following
project conventions for commits and PRs. Attach screenshots or recordings only
when they are supplied, for example by the `full-feature-flow` skill; it does not
prepare a runnable app or capture media itself.

Invocation authorizes the relevant commits, push to the feature branch, media
attachment and draft PR creation, including small fixes needed to pass hooks.
Honor narrower user constraints. Preserve unrelated changes. Do not merge,
mark a PR ready for review or push to the base branch. If the current branch is
the base branch, create a feature branch before committing or pushing. Do not
start, restart or stop apps or services.

## Workflow

**Inspect**

- Read repository instructions, contribution docs, PR templates and relevant
  scripts. Inspect the working tree, staged and unstaged changes, relevant
  untracked files and outgoing commits relative to the intended base branch.
  Identify the change's purpose and observable behavior.
- Establish the repository, origin, feature branch and PR base. Check for an
  existing PR for the branch to avoid creating a duplicate. Reuse it only as
  a draft; never change an existing ready PR's state without user approval.
- Project instructions define commit conventions. If they do not, inspect the
  four most recent commits and infer title style, language, scope and body
  conventions. For PR conventions, prefer project instructions and templates;
  otherwise inspect the four most recent PRs, including their titles and full
  bodies. Use all available examples when there are fewer than four.
- Collect supplied visual evidence from the preceding conversation or caller:
  file paths, captions, what each demonstrates and the source snapshot it was
  captured from, plus any app URL. Confirm each file exists and is readable.
  If none is supplied, continue without media; do not capture it.

**Commit and push**

- Stage only the intended changes and use ordinary `git commit` following the
  discovered project convention. Run ordinary `git push` to the feature branch
  on origin, setting the upstream when needed. Preserve Git hooks for both:
  never use `--no-verify`, commit `-n`, `HUSKY=0` or an equivalent bypass.
- If a hook fails, inspect its output, fix the actual issue within the change's
  scope and retry the failed operation. Small lint, formatting or generated-file
  fixes belong here. Stop and report issues requiring a scope or behavior
  change, unavailable credentials or unresolved external failures.
- Inspect the committed output and hook modifications. Include required generated
  files exactly as produced, preserving their order. Run required generation
  and checks on the final revision; reuse prior successful results only for
  identical content. Repeat until generation leaves no diff, rerunning only after
fixing a concrete cause; if unchanged inputs keep producing different output,
stop and report it as a blocker. Do not lose hook
  edits or leave them uncommitted; include them before the successful push.
- Recheck the remote branch before pushing and never overwrite remote commits
  without explicit authorization. On an ambiguous push failure, inspect remote
  state before retrying. Confirm the remote head matches the final local SHA.

**Publish**

- Compare the pushed source with the supplied media's source snapshot. If hook
  fixes changed the demonstrated behavior or relevant code, do not present that
  media as proof of the final version; omit it or label it as captured before
  those fixes, and report the mismatch.
- Always create the PR as a draft. Follow project title/body conventions or the
  four-PR pattern discovered earlier; preserve applicable template sections.
  Explain the concrete problem, resulting behavior and verification results,
  and disclose remaining issues or gaps.
- With supplied media, attach/upload it through a supported mechanism and embed
  the resulting repository-accessible media URLs with captions in the PR body.
  Local filesystem paths are not attachments. If upload requires an existing
  PR, create the draft first, then immediately attach the media and update its
  body. Do not invent an upload command or leave a placeholder as evidence.
  Without media, omit the visual evidence section; do not add placeholders.
- In T3 Code, immediately link each created or updated PR using
  `link_pull_request` with its full URL. Read back the PR to verify its draft
  state, base/head, final title/body and, when media was attached, that it is
  viewable by repository collaborators. If attachment fails, leave the PR
  draft and report the files and precise blocker.

## Result

Return the draft PR link, pushed commit SHA and branch, hook/check results,
fixes made, any remaining working-tree changes, attached media and what it
demonstrates (or that none was supplied) and remaining issues. Report success
only after the push and PR are verified, and any supplied media is attached or
its failure reported. If commit or push fails, state what completed and what
blocks publication; do not create a PR for an unpushed revision.

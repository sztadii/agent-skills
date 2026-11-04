---
name: commit-push
description: "Always use for Git commits or pushes, including within other skills. Preserve hooks and generated file order."
---

# Commit and Push

## Scope

Use for every commit or push. Honor repository rules, authorized scope and blockers;
commit-only does not authorize push, PR creation or merge. Preserve unrelated work.

## Dependencies

No skill dependencies

## Workflow

- Inspect changes and outgoing commits; verify branch and push destination.
- Never bypass hooks (`--no-verify`, commit `-n`, `HUSKY=0` or equivalents).
  Fix failures or stop.
- Inspect committed output and hook changes. Commit required generated files
  exactly as produced: ordering matters even when behavior is unchanged.
- Run required regeneration and checks on the final revision until generation
  leaves no diff. Hooks alone are insufficient. Check existing commits before push
  too; stop if blocked, out of scope or generation does not stabilize.
- Recheck remote head before push; never overwrite remote commits without
  authorization and a lease tied to that head. Confirm the pushed revision and
  follow PR-linking requirements. Inspect remote state before ambiguous retries.

## Result

Report commits, confirmed push destination/head, checks, remaining changes and blockers.

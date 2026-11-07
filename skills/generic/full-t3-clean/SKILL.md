---
name: full-t3-clean
description: "Clean up T3 Code and local repositories: archive every settled thread, remove every Git worktree and delete every local branch except the default one, discarding uncommitted work. Trigger only when the user explicitly invokes this skill."
---

# Full T3 Clean

## Scope

Reset the local workspace to a clean state. Everything worth keeping is on
`origin`, so local work is disposable:

1. Archive every settled T3 Code thread. Agents cannot delete threads and
   T3 Code never deletes archived ones on its own; only the user can, in the
   app.
2. Remove every Git worktree of the repositories under the roots and every
   T3 Code worktree under `~/worktrees`, including ones with uncommitted or
   unpushed changes.
3. In every main checkout under the roots, discard uncommitted changes, switch
   to the default branch, reset it to `origin` and delete every other local
   branch, merged or not.

The roots are `~/projects` and `~/kineti`: every direct child that is a Git
repository. The user may name other roots or narrow the scope.

Invocation authorizes all of the above without asking for confirmation, without
backups and in every T3 Code environment. It does not authorize touching
`origin`: never push, delete remote branches or close PRs. Never delete a
repository, a main checkout or files Git ignores (such as `.env` or
`node_modules`), and never unsettle, unarchive or edit threads. Leave this
run's own thread and the worktree it runs in untouched.

## Workflow

1. **Inventory.** Record this thread's id and its working directory. List the
   repositories under the roots with their default branch
   (`git symbolic-ref refs/remotes/origin/HEAD`, else `main` or `master` if it
   exists on `origin`). A repository without `origin` or a default branch is
   skipped and reported, since its work may exist only locally.
2. **Archive settled threads.** In every environment from
   `t3_environment_links`, page through `t3_thread_list` with `allProjects`
   and `settled: true` until no cursor remains, and archive each thread with
   `t3_thread_organize` (`action: "archive"`). Skip this run's thread. Record
   failures and continue.
3. **Remove worktrees.** For each repository, take every entry of
   `git worktree list --porcelain` except the main checkout and this run's
   working directory, and run `git worktree remove --force --force <path>`.
   Then remove every remaining directory under `~/worktrees/<project>/` that
   is not a registered worktree of any repository and not this run's working
   directory, run `git worktree prune` in each repository and delete empty
   project folders under `~/worktrees`.
4. **Reset main checkouts and delete branches.** In each main checkout run
   `git fetch --prune origin`, `git reset --hard`, `git clean -fd` (never
   `-x`), `git switch <default>` and `git reset --hard origin/<default>`. Then
   delete every other local branch with `git branch -D`, except a branch still
   checked out in this run's worktree.
5. **Verify.** Confirm that `t3_thread_list` with `settled: true` returns no
   unarchived threads besides this one, that each repository lists only its
   main checkout (and this run's worktree), that `~/worktrees` holds nothing
   else, and that `git branch` shows only the default branch, clean and equal
   to `origin/<default>`. Retry a failed step once, then report it.

## Result

Lead with whether the workspace is clean. Report the number of threads
archived, the worktrees removed per repository and the branches deleted per
repository, naming any that had uncommitted or unpushed work. List skipped
items with the reason: this run's thread and worktree, repositories without
`origin` and anything that failed after a retry. If the workflow stopped,
say which stage completed and what remains. End with a reminder that archived
threads stay stored until the user deletes them in the T3 Code app.

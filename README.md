# My global skills

Private history of skills installed globally with skills.sh.

This checkout lives at `~/.agents`. Track both `skills/` (actual contents)
and `.skill-lock.json` (source metadata). Git history preserves local edits
and lets you inspect or recover content overwritten by an installation/update.

## Install another skill normally

```sh
npx skills add OWNER/REPO -g
cd ~/.agents
git status --short
git diff
git add skills .skill-lock.json
git commit -m "Add skills from OWNER/REPO"
git push
```

## Edit a skill

Edit `~/.agents/skills/<name>/SKILL.md` or its supporting files, then review
and commit the changes as above. New files appear in `git status`; use
`git diff --cached` after staging to review them too.

## Update with a recovery point

Commit any local edits before running `npx skills update`. Updates can
replace customized files; Git does not merge them automatically. Afterwards
review `git diff` and deliberately keep, merge, or restore changes before
committing. `npx skills check` checks for available updates.

## Recover a file

```sh
cd ~/.agents
git log --oneline -- skills/<name>
git show <commit>:skills/<name>/SKILL.md
# After inspecting the old version:
git restore --source=<commit> -- skills/<name>/SKILL.md
```

Git records snapshots when you commit; it does not automatically record every
write. Commit before installations or updates when you have local changes.
Skills managed by application plugins outside `~/.agents` are not included.

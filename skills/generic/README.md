# Generic skills

[Back to main README](../../README.md)

## Skills

- [full-babysit-pr](full-babysit-pr/SKILL.md) — Maintains an existing PR: analyzes review feedback and CI, prepares simplified and verified fixes, pushes them after your approval, replies to and resolves threads, and merges once a human marks it ready and all requirements pass.
- [full-council](full-council/SKILL.md) — An Astra Moderator picks three categories for the code or problem from chat, has six Codex and Claude analysts examine them in pairs, and returns a synthesized report.
- [full-feature-flow](full-feature-flow/SKILL.md) — Agrees on a plan with a Codex Planner backed by parallel feature-conflict and impact analysts, then a Claude Coder implements it, has a Quality Leader check it with code reviewers, critical-issue reviewers and user flow testers until it passes (at most three rounds), and returns the app URL with screenshots or recordings of the final version.
- [full-review-pr](full-review-pr/SKILL.md) — Checks out a PR locally, has an Astra Moderator run eight Codex and Claude analysts in parallel for critical bugs, unnecessary duplication, inconsistencies with project rules and unmet Jira requirements, shows you the proposed comments and publishes the ones you accept with the full-babysit-pr attribution, skipping any that someone has already raised.
- [full-simplify](full-simplify/SKILL.md) — An Astra Moderator has six Codex and Claude analysts look for a smaller diff, redundant tests and reusable existing code in a change, validates that each proposal keeps behavior the same, and applies the best ones to the working tree without committing.
- [full-create-pr](full-create-pr/SKILL.md) — Commits and pushes with Git hooks, then creates a draft PR following project conventions, attaching supplied screenshots or recordings when available.

---
name: leaving-pr-comments
description: Trigger this if the user says 'leaving-pr-comments' or 'leaving pr comments', or another skill invokes it.
disable-model-invocation: true
---

# Leaving PR Comments

Prefix every agent-authored comment left on the user's behalf with this
attribution line:

```md
🤖 **<model> responding on behalf of <user>**

Actual comment starts here.
```

Use the verified public name of the model that wrote the comment, and the
user's name or GitHub login. If the harness does not expose a model name,
use the harness name. Do not guess.

Before posting, check the entire comment, including quoted text and
attachments, for secrets and private details.

Keep the attribution on its own line, followed by the reply. Do not add a
callout, blockquote, heading, signature footer, or second attribution.

Apply this format to top-level PR comments, inline review comments, replies to
bots or reviewers, and written explanations used before resolving or
dismissing feedback. Do not add it to PR descriptions, commit messages, or text
the user explicitly asked to post verbatim without attribution.

Write the comment itself in a direct style:

- Lead with the conclusion.
- Cite the relevant code, behavior, check, or reproduction.
- Use one sentence whenever possible, two at most (excluding the attribution).
  Include only what the reviewer needs; avoid filler, canned thanks, and status
  narration.
- Distinguish verified facts from judgment.

Preview the final Markdown before posting so quoting and blank lines render as
shown. After posting, verify that GitHub returned the comment or review URL.
Resolve a review thread only after its response was posted successfully.

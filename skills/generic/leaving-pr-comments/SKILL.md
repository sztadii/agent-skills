---
name: leaving-pr-comments
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Leaving PR Comments

## Scope

Draft, rewrite or publish requested PR messages without a comprehensive review or code
edits. Requests to post or handle discussions, including from a calling workflow,
authorize publication and resolution of clearly settled threads. Drafts and
rewrites return text only. Honor narrower caller limits.

For supplied-text rewrites, preserve meaning and return text directly, without
remote lookup, new facts or attribution unless requested. Otherwise follow the
relevant steps below.

## Dependencies

No skill dependencies

## Workflow

**Prepare**

Read the current head, relevant diff, comments and thread history; verify claims
against current code and evidence. Each finding needs a trigger, consequence and
useful next step. Omit speculative, duplicate or fixed findings; prefer one issue
per comment and separate observations from judgment.

Limit each comment or reply, including drafts and rewrites, to two sentences,
excluding the attribution prefix. Choose the prefix based on the message's origin.

For text the user previously sent or explicitly approved in its final form, use:

```md
🤖 <model>, loyal servant of <user>. This message was confirmed by master.

Actual comment starts here.
```

For a message the bot writes independently, use:

```md
🤖 <model>, loyal servant of <user>. This message was not confirmed by master.

Actual comment starts here.
```

Permission to post or act on the user's behalf does not establish approval of the
message's text. “Post that comment” after a displayed draft does approve its text.
Use the confirmed prefix when the conversation, verified message history or calling
workflow establishes text approval, replacing any stale unconfirmed draft prefix
without asking again. Substantive rewrites need renewed text approval to retain
the confirmed prefix. Without text approval, use the unconfirmed prefix.

Use the host's model identity in a readable display form; include `OpenAI` before
Sol model names (for example, `gpt-6.1-sol` becomes `OpenAI 6.1 Sol`). Use the host
name if the model identity is unavailable, and a verified user name or authenticated
GitHub login; never guess. Omit attribution from PR descriptions, commit messages
and text requested verbatim. Check text and attachments for private data and formatting.

**Publish**

- Recheck head and relevance. Anchor findings to valid diff lines, reply in
  existing threads and post summaries as top-level comments. Report unanchorable
  inline-only messages without changing their format.
- Publish authorized messages without another approval. Use COMMENT reviews unless
  a formal decision was requested. Inspect remote state before retrying ambiguous
  responses or timeouts to avoid duplicates.
- Verify the published body, confirmation prefix and URL.
- Resolve handled threads without another request when current code and discussion
  clearly settle every concern. Verify required changes and successful publication
  of a reply explaining the fix or withdrawal of the finding. The concern raiser's
  acceptance, such as “makes sense” or a thumbs-up on that reply, supports resolution
  if no questions or objections remain. Verify who reacted and to which message;
  unrelated reactions or others' approvals do not establish their acceptance.
- Leave threads open if intent, required changes or acceptance is unclear. Silence,
  outdated diffs, an author's “fixed” reply and green CI alone do not prove resolution.
  Manage only this workflow's threads unless explicitly authorized otherwise;
  verify the resolved state after closing.

## Result

Reply in at most two sentences with posted URLs, requested drafts, thread states
and reasons for withheld actions. Merging, PR status changes and ongoing monitoring
require scope from the caller's request.

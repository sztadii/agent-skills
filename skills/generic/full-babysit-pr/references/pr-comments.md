# PR comments

Reply to handled feedback without another approval; resolve only threads with
trivial, indisputable fixes.

**Prepare**

Read the current head, relevant diff, comments and thread history; verify claims
against current code and evidence. Omit speculative, duplicate or already
answered points; separate observations from judgment.

Limit each reply to two sentences, excluding the attribution prefix. Start each
with the confirmed prefix when the user approved this message or the change it
reports; otherwise use the automatic prefix. `<comment>` marks where the message
text goes and is not published.

Confirmed:

```md
🤖 <model>, loyal servant of <user>. My master read this, nodded wisely and let me send it.

<comment>
```

Automatic:

```md
🤖 <model>, loyal servant of <user>. I wrote this while my master wasn't looking.

<comment>
```

Use the host's model identity in a readable display form; include `OpenAI` before
Sol model names (for example, `gpt-6.1-sol` becomes `OpenAI 6.1 Sol`). Use the host
name if the model identity is unavailable, and a verified user name or authenticated
GitHub login; never guess. Omit attribution from PR descriptions and commit
messages. Check text and attachments for private data and formatting.

**Publish**

- Recheck head and relevance. Reply in existing threads; post summaries as
  top-level comments. Use COMMENT reviews unless a formal decision was requested.
  Inspect remote state before retrying ambiguous responses or timeouts to avoid
  duplicates.
- Verify the published body, prefix and URL.
- Resolve a thread only when its fix is published and so trivial that nobody
  could reasonably dispute it: for example a typo, a rename or a change applied
  exactly as the reviewer suggested it. Verify the change and the published reply
  first.
- Never resolve when any judgment was involved: the reviewer described a problem
  but you chose the solution, the fix differs from the suggestion, you explain a
  false positive or deferral, or the change has alternatives. Reply and leave
  these threads open for the reviewer, even after acceptance such as “makes sense”.
- When in doubt, leave the thread open. Silence, outdated diffs, an author's
  “fixed” reply and green CI alone do not justify resolution. Manage only this
  workflow's threads unless explicitly authorized otherwise; verify the resolved
  state after closing.

Record posted URLs, thread states and reasons for withheld actions.

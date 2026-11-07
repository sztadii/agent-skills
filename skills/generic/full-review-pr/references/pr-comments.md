# PR comments

Draft and publish review comments for accepted findings only.

**Draft**

- One comment per finding, anchored to the most relevant changed line; use a
  top-level comment only when no line fits. Limit each to two sentences,
  excluding the attribution prefix: the problem with its consequence, then the
  suggested fix. Separate observations from judgment.
- Start each with this prefix; `<comment>` marks where the message text goes
  and is not published:

```md
🤖 <model>, loyal servant of <user>. My master read this, nodded wisely and let me send it.

<comment>
```

- Use the host's model identity in a readable display form; include `OpenAI`
  before Sol model names (for example, `gpt-6.1-sol` becomes `OpenAI 6.1 Sol`).
  Use the host name if the model identity is unavailable, and a verified user
  name or authenticated GitHub login; never guess. Check text for private data
  and formatting.

**Publish**

- Post all accepted comments as inline comments of one COMMENT review against
  the reviewed head SHA. Never approve or request changes.
- Inspect remote state before retrying ambiguous responses or timeouts to avoid
  duplicates.
- Verify each published body, prefix and URL. Record posted URLs and the
  reasons for skipped comments.

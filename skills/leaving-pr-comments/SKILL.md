---
name: leaving-pr-comments
description: Trigger this if the user says 'leaving-pr-comments' or 'leaving pr comments', or another skill invokes it.
disable-model-invocation: true
---

# Leaving PR Comments

For authorized comments on the user's behalf, use this prefix on its own line:

```md
🤖 **<model> responding on behalf of <user>**

Actual comment starts here.
```

Use the verified public model name (harness name if unavailable) and verified
user name or GitHub login; never guess. Apply to top-level/inline comments and
replies, including explanations before resolving feedback. Exclude PR descriptions,
commits and text explicitly requested verbatim without attribution. Do not add
another attribution, wrapper or footer.

Lead with the conclusion and cite relevant code, behavior or checks. Distinguish
facts from judgment; prefer one or two sentences without filler.
Check the complete Markdown and attachments for private data and formatting.
After posting, verify the returned comment/review URL. Resolve a thread only
after its response posts successfully. This skill does not authorize publication.

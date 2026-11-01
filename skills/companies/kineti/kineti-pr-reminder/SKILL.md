---
name: kineti-pr-reminder
description: "Trigger only when the user or another skill explicitly invokes this skill to send Kineti PR review reminders on Slack."
---

# Kineti PR review reminders

Run only when explicitly invoked with `$kineti-pr-reminder` (Codex),
`/kineti-pr-reminder` (Claude Code), or an instruction from another skill.
Mentions, examples and matching tasks alone do not trigger this workflow.
Scheduled invocations must explicitly name this skill.

On each invocation, check the user's current PRs and send the team a concise
Slack reminder. Invoking this workflow authorizes sending the reminder; do not
ask again for permission to send to the established destination. Creating or
editing the skill does not invoke it.

## Establish context

- Identify the user through the authenticated GitHub account (for example,
  `gh api user --jq .login`), rather than guessing from local git configuration.
- Determine the Kineti organization and repositories from the invocation,
  project configuration or confirmed GitHub context. Keep the search within
  that scope.
- Determine the Slack workspace and channel from the instructions or an
  unambiguous, previously established context. Use available read tools to
  resolve the destination when helpful. If the repository scope or recipient
  remains ambiguous, ask only for the missing information. In an automated run,
  report the blocker without sending to an arbitrary channel.

## Check PRs

1. Fetch open PRs authored by the authenticated user within the established
   scope. Handle pagination.
2. Check the current state, draft status, review decision, requested reviewers
   and reviews for the current revision. Include PRs that are ready for review
   and still need it. Skip drafts, closed or merged PRs, and PRs with the required
   approvals, unless the user specifies a different scope.
3. Collect each selected PR's title, number, repository and URL. Do not claim
   that CI passed or someone is blocking progress unless the data supports it.

## Send the reminder

- Send one short, consolidated message in Polish to the established Slack
  channel: a request for review and a list of links with titles and repositories.
  Adapt the message if the team uses a different established language.
- Do not add `@channel`, `@here` or guessed user mentions. Mention reviewers only
  with confirmed account mappings and an established team practice or user
  instruction.
- If no PR needs review, send no empty reminder; report the result in the reply.
- If available channel history already contains an identical reminder from this
  run, skip the duplicate. After an ambiguous send result, check history before
  retrying; do not resend blindly.
- Use an available Slack tool to send the message. Report missing access as a
  blocker rather than presenting a draft as a sent message.
- Finish with the number of included PRs and a link to the sent message if the
  tool returned one. Clearly report omissions or incomplete data that limited
  the result.

This skill performs one check and send per invocation. Creating or changing
schedules requires a separate user instruction.

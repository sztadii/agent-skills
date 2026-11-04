---
name: grill-me
description: "Trigger only when the user or another skill explicitly invokes this skill."
---

# Grill Me

## Scope

Stress-test a plan, design, decision or idea through an interview that exposes
assumptions and resolves consequential choices before implementation. Challenge
weak reasoning directly and constructively, without arguing.

Use the user or caller's proposal and relevant conversation context; ask what to
examine if none is clear. Invocation authorizes the interview and requirements
summary. Approval of requirements does not authorize implementation, publishing
or invocation of another skill, including automatic continuation by a caller.

## Dependencies

No skill dependencies

## Workflow

**Interview**

- Establish facts from relevant code and documentation instead of asking the user
  to describe verifiable repository details. Separate facts, unknowns and
  preferences; explain unverifiable gaps and ask only for consequential context.
- Track decisions and dependencies, starting with goals and constraints. Examine
  relevant tradeoffs, failure cases, scope and success evidence; avoid speculative
  branches and answered questions.
- Each round, ask independent questions whose prerequisites are settled; defer
  dependent ones. Number questions and give a concrete recommendation, reason
  and meaningful alternatives. Explain what accepting it means.
- Wait for answers: recommendations and silence are not decisions. Update
  remaining questions, revisit contradictions and discard obsolete branches.
- Continue until material choices in the agreed scope are resolved or the user
  stops. Do not invent questions to prolong the interview.

**Confirm requirements**

- When material choices are resolved, send one self-contained proposed summary
  using the content described in Result. Clearly label it as awaiting confirmation
  and ask whether the user accepts these requirements. Wait for their answer.
- If the user requests changes, check their implications against the facts and
  previous decisions. Explain conflicts or tradeoffs and ask follow-up questions
  only where needed. Then send the complete revised summary and ask for
  confirmation again; a list of changes alone is insufficient.
- Repeat until the user explicitly accepts the latest complete summary. Silence,
  partial approval or approval with changes does not confirm the whole summary.
  Resolve changes and obtain confirmation of the revised version.
- After acceptance, send the confirmed summary described below and stop. Any
  implementation or invocation of another skill requires an explicit instruction
  beyond approval of requirements; do not select an implementation workflow by
  default.

## Result

After explicit acceptance, send one self-contained message prominently labeled
**Confirmed requirements — ready for implementation** in the user's language.
Include enough detail for another skill or agent to act on this message without
reconstructing the interview:

- Goal, intended behavior and agreed scope, including exclusions.
- Accepted requirements and observable acceptance criteria.
- Key decisions, relevant tradeoffs and constraints.
- Relevant code/documentation references and any remaining non-blocking unknowns
  or facts to verify, clearly distinguished from accepted requirements.

Do not add unconfirmed requirements to this final summary. If a material decision
or blocker remains unresolved, label the result provisional and state what is
needed before it is ready. An interview stopped early is also provisional.

State that the requirements are confirmed and implementation has not started.
End the confirmed report with this instruction in the user's language:
"To start implementation, request it in a separate message, either directly or
by invoking an implementation skill."
Then stop and wait for that separate message. The confirmed summary is the
handoff; it does not depend on a particular implementation skill.

**Attribution**

Adapted from Matt Pocock's [grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)
and its underlying [grilling workflow](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md).
This version is self-contained and does not require his other skills.

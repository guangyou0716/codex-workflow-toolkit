---
name: chat-handoff-summary
description: "Create a concise, copyable continuation handoff from the current conversation, preserving the goal, confirmed decisions, constraints, technical details, files, skills, paths, completed work, pending tasks, open questions, rejected approaches, and exact next action. Use when the user asks to summarize a chat for a new chat, prepare a handoff, extract key points, or create a continuation prompt."
---

# Chat Handoff Summary

Produce a standalone handoff that lets a capable assistant continue the work
without access to the original conversation. Summarize only the current
conversation. Do not continue implementing, researching, editing, or solving
the original task.

## Source of truth

- Use only information available in the current conversation, including visible
  user messages, assistant messages, tool results, and relevant supplied files.
- Do not invent missing details, turn unresolved decisions into confirmed ones,
  silently correct earlier statements, or replace the conversation's
  terminology with unrelated terminology.
- Do not include unrelated profile information, irrelevant older context, or
  background memories that were not relevant to this conversation.
- Do not use web research unless the user explicitly asks for verification.
- When messages conflict, prefer the latest explicit user decision. Mention an
  earlier option only when it still matters, and label unresolved conflicts.

## Extract and classify

Capture information that affects future work:

- the user's main goal and relevant background;
- requirements, preferences, constraints, and accepted recommendations;
- project, repository, environment, model, reasoning, tools, skills, and
  workflow details;
- exact file paths, directory paths, filenames, commands, prompts,
  configuration keys, branch names, endpoints, and report or plan locations;
- work already completed and evidence for completion;
- pending actions, unresolved questions, known limitations, mistakes, and
  rejected approaches.

Classify important items explicitly:

- **Confirmed** â€” explicitly decided or accepted by the user.
- **Recommended** â€” suggested by the assistant but not clearly confirmed.
- **Pending** â€” unresolved decision, action, or question.
- **Rejected or superseded** â€” an earlier approach that must not be repeated.

Do not present recommendations as confirmed decisions.

## Preserve technical and file details

- Keep exact technical identifiers in code formatting when they could be
  copied or executed.
- For a relevant uploaded or generated file, include its exact visible
  filename and purpose. Distinguish uploaded files from generated files.
- Do not invent a local path. If the next chat may need an uploaded file again,
  say so.
- Never reproduce passwords, access tokens, private keys, full credential
  values, or unrelated sensitive information. State only that a credential or
  configuration is required.

## Required output

Use exactly the following structure and heading order:

```markdown
# New Chat Handoff

## Objective

## Current Context

## Confirmed Decisions

## Files, Skills, and Paths

## Agreed Workflow

## Models and Reasoning

## Important Rules and Constraints

## Work Already Completed

## Pending Tasks

## Open Questions

## Rejected or Superseded Approaches

## Continue From Here

## Copyable Continuation Prompt
```

Fill each section with only relevant information:

- **Objective:** State the main goal in one to three sentences.
- **Current Context:** Summarize the environment, project, workflow, and
  background needed to continue.
- **Confirmed Decisions:** List settled decisions and exact technical details.
- **Files, Skills, and Paths:** Explain the purpose of each relevant item.
- **Agreed Workflow:** Describe the intended execution order; use numbered
  steps when order matters.
- **Models and Reasoning:** Separate confirmed defaults, situation-specific
  alternatives, and unconfirmed recommendations.
- **Important Rules and Constraints:** Include coding, Git, validation,
  privacy, tool, and communication rules that the next assistant must obey.
- **Work Already Completed:** Report only work supported by the conversation;
  do not claim completion without evidence.
- **Pending Tasks:** List remaining actions in practical order. For each, state
  the expected output, required skill or model when known, and any blocker.
- **Open Questions:** List only genuinely unresolved questions. Write `None`
  when there are none.
- **Rejected or Superseded Approaches:** Record approaches that should not be
  repeated. Write `None` when not applicable.
- **Continue From Here:** Give one clear instruction for the next assistant,
  including the exact next action.

## Copyable continuation prompt

Write a self-contained prompt that does not depend on unavailable earlier
messages. It must:

1. Begin exactly with:

   `Continue this task using the following context:`

2. Include the necessary objective, context, decisions, exact paths and skill
   names, constraints, completed work, and remaining work.
3. State the next requested action clearly.
4. Avoid claims that the new chat automatically has access to the old chat.
5. End with a concrete next action, not an open-ended request such as
   "Please help me continue."

## Length and final behavior

Adapt the handoff to the conversation:

- simple: approximately 300â€“700 words;
- medium technical: approximately 700â€“1,500 words;
- long, multi-stage technical: approximately 1,500â€“3,000 words.

Prefer completeness over extreme brevity, but remove repetition and filler.
After producing the handoff, stop. Do not resume the original task, add
unrelated recommendations, or ask follow-up questions unless the handoff
request itself is ambiguous.


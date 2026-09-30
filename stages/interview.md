---
name: interview
description: A relentless interview that sharpens an idea into a settled design tree — grounded by parallel codebase exploration first, and kept entirely in conversation.
---

# Interview

Explore the ground, then interview the user relentlessly until you reach a shared understanding. Map the result as a **design tree**: every decision branches into the decisions that hang off it. This stage writes nothing down — the tree lives in the conversation, and its settled decisions and vocabulary flow into the plan.

## Explore before you ask

If the feature touches code you haven't read — an unfamiliar module, someone else's repo, an area you'd otherwise be guessing about — dispatch **two or three explore sub-agents in parallel** before composing round one, then read the key files they surface yourself:

1. **Similar features** — find features resembling this request and trace how they are implemented end to end.
2. **The lay of the land** — map the architecture and abstractions of the area the feature will touch.
3. **The status quo** — analyze how the behaviour being changed or replaced works today.

Launch first, ask second: their facts feed your recommended answers, so have them running while you draft round one. If you wrote the surrounding code yourself and know it cold, skip this — exploration you don't need is ceremony.

## Rounds and the frontier

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them and wait.

## Depth control

- **standard tier:** one round is usually enough. Settle the top-level decisions and their immediate consequences, then move to the plan — do not expand every leaf of the tree. Take a second round only when its answers would clearly change the plan's shape; otherwise carry open micro-decisions into implementation.
- **full tier:** work until the frontier is empty — every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Domain discipline, in conversation

Sharpen the project's language as you design — just don't write files to do it:

- **Challenge settled vocabulary.** When the user uses a term that conflicts with language already established in this conversation, call it out immediately. "You defined 'cancellation' as X, but you seem to mean Y now — which is it?"
- **Sharpen fuzzy language.** When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."
- **Discuss concrete scenarios.** Stress-test domain relationships with specific scenarios that probe edge cases and force precision about the boundaries between concepts.
- **Cross-reference with code.** When the user states how something works, check whether the code agrees; surface any contradiction.

Terms that crystallise land in the plan's Decisions section — that is where they get written down, and nowhere else.

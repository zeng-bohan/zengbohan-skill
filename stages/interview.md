---
name: interview
description: A relentless interview that sharpens an idea into a settled design tree, leaving a glossary (CONTEXT.md) and ADRs behind as it goes.
---

# Interview

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it. As terms and hard-to-reverse choices crystallise, leave a paper trail (see Domain modeling below).

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

## Domain modeling — leave a paper trail

Actively build and sharpen the project's domain model as you design: challenge terms, invent edge-case scenarios, and write the glossary and decisions down the moment they crystallise. (Merely _reading_ `CONTEXT.md` for vocabulary is not this — that's a one-line habit. This is for when you're changing the model, not just consuming it.)

Most repos have a single context: `CONTEXT.md` and `docs/adr/` at the repo root. If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts and the map points to each one's `CONTEXT.md` and `docs/adr/`. Create files lazily — only when you have something to write.

During the session:

- **Challenge against the glossary.** When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"
- **Sharpen fuzzy language.** When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."
- **Discuss concrete scenarios.** Stress-test domain relationships with specific scenarios that probe edge cases and force precision about the boundaries between concepts.
- **Cross-reference with code.** When the user states how something works, check whether the code agrees; surface any contradiction.
- **Update `CONTEXT.md` inline** the moment a term resolves — don't batch. Use the format in `../references/CONTEXT-FORMAT.md`. `CONTEXT.md` is a glossary and nothing else: no implementation details, no specs, no scratch pad.
- **Offer ADRs sparingly** — only when all three are true: hard to reverse, surprising without context, and the result of a real trade-off. Use the format in `../references/ADR-FORMAT.md`.

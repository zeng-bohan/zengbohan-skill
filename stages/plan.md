---
name: plan
description: Synthesize settled decisions into a plan — a one-pager with a single approval (standard), or a published spec plus tracer-bullet tickets (full). No interview; pure synthesis of what you already discussed.
---

# Plan

Take the current conversation and codebase understanding and turn them into a plan. Do NOT interview the user — just synthesize what you already know. Use the project's domain glossary from `CONTEXT.md` throughout; respect ADRs in the area you're touching. Explore the repo first if you haven't already.

In both modes, sketch the **seams** at which the feature will be tested: prefer existing seams over new ones, the highest seam possible, the fewest total (the ideal is one). Seams are confirmed as part of the one plan approval — no separate sign-off.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Decision-rich parts only — not a working demo.

## Standard mode — plan file, one gate

Write the plan to `.scratch/<feature-slug>/plan.md` (create the directory if needed). The file — not the conversation — is the durable record: it is what a fresh session resumes from and what code-review reads later. Present its contents in the conversation and iterate by editing the file.

<plan-template>

## Problem & Stories

The problem, from the user's perspective, in a sentence or two — then 2–4 user stories in the form "As an <actor>, I want <feature>, so that <benefit>". This is what final acceptance checks against.

## Decisions

The settled implementation decisions: modules built/modified, interface changes, schema changes, API contracts, architectural choices, technical clarifications.

## Seams

The seams agreed for testing, and what each covers.

## Tickets

One checkbox per ticket, numbered in dependency order (blockers first). For each ticket:

- [ ] **T1 — <Title>** · **Blocked by**: T2, or "None — can start immediately" · **Delivers**: the end-to-end behaviour this ticket makes work, from the user's perspective — not a layer-by-layer implementation list · **Verify**: the one command or interaction that shows it works

Each ticket is a **tracer bullet**: a narrow but complete path through every layer (schema, API, UI, tests), demoable or verifiable on its own, sized to fit in one context window. Wide refactors (mechanical changes with codebase-wide blast radius) are sequenced expand–contract instead — see Full mode for the rules; they apply here too.

## Out of Scope

What this plan deliberately does not touch.

</plan-template>

Present the plan once and ask one question: does this look right — problem, stories, seams, ticket granularity, and blocking edges? Iterate on whatever they push back on, then proceed. **This single exchange is the only gate** — do not re-ask piecemeal afterwards. No issue tracker needed unless the user asks.

## Full mode — spec, then tickets

Full mode produces durable artifacts for multi-session work: a published spec, then published tickets. Both need the issue tracker from `docs/agents/issue-tracker.md` — if it is missing, run `stages/setup.md` first.

### 1. Spec

Write the spec with the template below, then present it once — Testing Decisions included, so the seams are confirmed in the same exchange — and publish on confirmation to the issue tracker with the `ready-for-agent` label (see `../references/triage-labels.md` for the label strings).

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

Cover every aspect the feature actually touches — long for a large feature, a handful of stories for a small one. No filler.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- The seams at which the feature will be tested (prefer existing, highest possible, fewest total)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>

### 2. Tickets

Break the spec into **tracer-bullet tickets** — vertical slices, each declaring its **blocking edges** (the other tickets that must complete before it can start; a ticket with no blockers can start immediately).

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests) — vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Any prefactoring should be done first ("make the change easy, then make the easy change")

</vertical-slice-rules>

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change — rename a column, retype a shared symbol — whose **blast radius** fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a tracer bullet; sequence it as **expand–contract**. First expand: add the new form beside the old so nothing breaks. Then migrate the call sites over in batches sized by blast radius (per package, per directory), each batch its own ticket blocked by the expand, keeping CI green batch to batch because the old form still exists. Finally contract: delete the old form once no caller remains, in a ticket blocked by every migrate batch. When even the batches can't stay green alone, keep the sequence but let them share an integration branch that all block a final integrate-and-verify ticket — green is promised only there.

**Quiz the user.** Present the proposed breakdown as a numbered list (Title / Blocked by / What it delivers) and ask:

- Does the granularity feel right? (too coarse / too fine)
- Are the blocking edges correct — does each ticket only depend on tickets that genuinely gate it?
- Should any tickets be merged or split further?

Iterate until the user approves the breakdown.

**Publish** the approved tickets in dependency order (blockers first) to the configured tracker:

- **Local files** → one file per ticket under `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` (template below) — never a single combined file.
- **A real issue tracker (GitHub, GitLab, …)** → one issue per ticket, using the tracker's native blocking mechanism (see `docs/agents/issue-tracker.md`). Apply the `ready-for-agent` label.

Do NOT close or modify any parent issue.

<local-ticket-template>

# <NN> — <Ticket title>

**What to build:** the end-to-end behaviour this ticket makes work, from the user's perspective — not a layer-by-layer implementation list.

**Blocked by:** the numbers/titles of the tickets that gate this one, or "None — can start immediately".

**Status:** ready-for-agent

- [ ] Acceptance criterion 1
- [ ] Acceptance criterion 2

</local-ticket-template>

<issue-template>

## Parent

A reference to the parent issue on the tracker (if the source was an existing issue, otherwise omit this section).

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective — not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Blocked by

- A reference to each blocking ticket, or "None — can start immediately".

</issue-template>

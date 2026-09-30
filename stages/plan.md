---
name: plan
description: Synthesize settled decisions into a plan file — the pipeline's only durable artifact. One approval (standard) or two gates, content then slice breakdown (full). No interview; pure synthesis of what you already discussed.
---

# Plan

Take the current conversation and codebase understanding and turn them into a plan. Do NOT interview the user — just synthesize what you already know. Use the vocabulary settled in the interview throughout. Explore the repo first if you haven't already.

In both modes, sketch the **seams** at which the feature will be tested: prefer existing seams over new ones, the highest seam possible, the fewest total (the ideal is one). Seams are confirmed as part of the plan approval — no separate sign-off.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Decision-rich parts only — not a working demo.

## The plan file

Write the plan to `.scratch/<feature-slug>/plan.md` (create the directory if needed). This file is the pipeline's **only durable artifact**: it is what a fresh session resumes from, what implement ticks off slice by slice, and what code-review reads later. Present its contents in the conversation and iterate by editing the file. Everything else — reasoning, rejected options, vocabulary debates — stays in the conversation.

<plan-template>

## Problem & Stories

The problem, from the user's perspective, in a sentence or two — then 2–4 user stories in the form "As an <actor>, I want <feature>, so that <benefit>". This is what final acceptance checks against.

## Decisions

The settled implementation decisions: modules built/modified, interface changes, schema changes, API contracts, architectural choices, technical clarifications — and the canonical terms the interview settled, so the vocabulary survives into later sessions.

## Seams

The seams agreed for testing, and what each covers.

## Slices

One checkbox per slice, numbered in dependency order (blockers first). For each slice:

- [ ] **S1 — <Title>** · **Blocked by**: S2, or "None — can start immediately" · **Delivers**: the end-to-end behaviour this slice makes work, from the user's perspective — not a layer-by-layer implementation list · **Verify**: the one command or interaction that shows it works

Each slice is a **tracer bullet**: a narrow but complete path through every layer (schema, API, UI, tests), demoable or verifiable on its own, sized to fit in one context window.

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change — rename a column, retype a shared symbol — whose blast radius fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a tracer bullet; sequence it as **expand–contract**. First expand: add the new form beside the old so nothing breaks. Then migrate the call sites over in batches sized by blast radius (per package, per directory), each batch its own slice blocked by the expand, keeping CI green batch to batch because the old form still exists. Finally contract: delete the old form once no caller remains, in a slice blocked by every migrate batch. When even the batches can't stay green alone, keep the sequence but let them share an integration branch that all block a final integrate-and-verify slice — green is promised only there.

## Out of Scope

What this plan deliberately does not touch.

</plan-template>

## Standard mode — one gate

Present the plan once and ask one question: does this look right — problem, stories, seams, slice granularity, and blocking edges? Iterate on whatever they push back on, then proceed. **This single exchange is the only gate** — do not re-ask piecemeal afterwards.

## Full mode — two gates, one file

Full mode is for work that will span sessions or days. Nothing new is published — the same plan file is walked in two passes, giving multi-session work an extra steering point before any code moves:

1. **Content gate.** Present problem, stories, decisions, seams, and out of scope (leave Slices blank for now). Iterate until settled.
2. **Slice gate.** Fill in the Slices section, present it, and quiz the user:

   - Does the granularity feel right? (too coarse / too fine)
   - Are the blocking edges correct — does each slice only depend on slices that genuinely gate it?
   - Should any slices be merged or split further?

   Iterate until approved.

The approved plan file is the whole contract — there is no tracker, no spec, and no separate ticket files to keep in sync.

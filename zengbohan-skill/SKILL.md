---
name: zengbohan-skill
description: "The single entry point for every development task. Drives any new or continued implementation through the fixed five-stage pipeline: Grill-With-Docs (interview until the design tree is settled) -> to-spec (synthesize the spec) -> to-tickets (split into tracer-bullet tickets with blocking edges) -> implement (build via tdd) -> code-review (two-axis review). Use whenever the user starts a development task, wants to implement a feature, says 开发/实现/开始做/按流程走, or continues an in-progress build — even if they never say 'workflow'. This is the mandatory pipeline for all dev tasks; do not invent an ad-hoc process instead."
---

# zengbohan-skill

The mandatory pipeline for every development task. Walk the work through five stages in order, one unbroken banner at a time. Do not skip stages, do not merge stages, and do not treat an unpublished draft as a settled output.

This skill is self-contained: every stage definition it drives lives under `stages/` in this folder. It has no external skill dependencies — install this one folder and nothing else.

Phase-break and context rules follow ask-matt (see `stages/ask-matt/ask-matt.md`): keep stages 1–3 in one context window so the interview, spec, and tickets build on the same thinking; each `/implement` then starts fresh from its ticket.

## Stage 0 — Precondition check (once)

Before the first stage, confirm the issue tracker is configured for this repo:

- If `docs/agents/issue-tracker.md` exists, read it and proceed.
- If it is missing, run setup-matt-pocock-skills (read `stages/setup-matt-pocock-skills/setup-matt-pocock-skills.md`) to configure the issue tracker, triage label vocabulary, and domain doc layout. Confirm each configuration choice with the user before writing.

## Stage 1 — Grill-With-Docs

Sharpen the idea into a settled design tree, leaving a paper trail.

- Load `stages/grill-with-docs/grill-with-docs.md` (which runs `stages/grilling/grilling.md` and `stages/domain-modeling/domain-modeling.md`).
- Interview the user in rounds. Each round, ask the whole frontier — every decision whose prerequisites are settled — numbered, each with a recommended answer. Then **stop and wait** for the answers before the next round.
- Facts are your job, never the user's: when a frontier question needs an answer from the environment (filesystem, tools, docs, DB), look it up or dispatch a sub-agent — don't put it to the user. Only the *decisions* go to the user.
- Resolve terms into `CONTEXT.md` and record hard-to-reverse choices as ADRs as they crystallise (see `stages/domain-modeling/domain-modeling.md`).
- **Stage exit:** the frontier is empty — every branch visited, nothing silently assumed. Confirm with the user that you have a shared understanding before moving on.

## Stage 2 — to-spec

Turn the conversation and settled design into a spec — synthesis, not a new interview.

- Load `stages/to-spec/to-spec.md`.
- Use the project's domain glossary vocabulary from `CONTEXT.md`; respect ADRs in the area.
- Sketch the seams at which the feature will be tested (prefer existing seams, highest seam possible, fewest total). **Check with the user that these seams match their expectations.**
- Write the spec using the template in `stages/to-spec/to-spec.md` (Problem Statement, Solution, User Stories, Implementation Decisions, Testing Decisions, Out of Scope, Further Notes). No file paths or code snippets unless a prototype encoded a decision precisely.
- Publish it to the issue tracker with the `ready-for-agent` label (per `docs/agents/issue-tracker.md`).
- **Stage exit:** spec published and user has signed off on the seams.

## Stage 3 — to-tickets

Break the spec into tracer-bullet vertical slices, each declaring its blocking edges.

- Load `stages/to-tickets/to-tickets.md`.
- Each ticket cuts a narrow but complete path through every layer; a completed ticket is demoable/verifiable on its own; each fits in a single fresh context window. Sequence wide refactors as expand–contract, not as a vertical slice.
- **Quiz the user**: present the breakdown as a numbered list (Title / Blocked by / What it delivers) and ask whether granularity feels right, whether blocking edges are correct, and whether any should merge or split. Iterate until approved.
- Publish to the tracker in dependency order (blockers first), using the tracker's blocking mechanism (local `.scratch/<feature-slug>/issues/NN-*.md` files, or native links on a real tracker). Apply the `ready-for-agent` label.
- **Stage exit:** tickets approved by the user and published.

## Stage 4 — implement

Build each ticket.

- Load `stages/implement/implement.md`.
- Work the frontier: any ticket whose blockers are all done. Pick up tickets one at a time, each in a fresh context window seeded from the ticket file.
- Use TDD at the pre-agreed seams (read `stages/tdd/tdd.md`), one red-green slice at a time. Run typechecking and single-test-file regularly; run the full test suite once at the end.
- Commit each completed ticket to the current branch.
- **Stage exit:** all tickets built, committed, full test suite green.

## Stage 5 — code-review

Review the whole change along two independent axes.

- Load `stages/code-review/code-review.md`.
- Pin the fixed point (branch start / merge-base / pre-feature commit), confirm it resolves and the diff is non-empty.
- Point the Standards sub-agent at any documented coding standards plus the smell baseline from `stages/code-review/code-review.md`; point the Spec sub-agent at the spec/tickets. Run them in parallel, then aggregate verbatim under `## Standards` and `## Spec`.
- Fix any findings, re-run the review until both axes are clean.
- **Stage exit:** both axes report no blocking findings and the user is happy to hand off.

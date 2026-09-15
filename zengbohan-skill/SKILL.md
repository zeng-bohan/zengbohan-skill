---
name: zengbohan-skill
description: "Five-stage pipeline for feature-sized development: Grill-With-Docs (interview until the design tree is settled) -> to-spec (synthesize the spec) -> to-tickets (split into tracer-bullet tickets with blocking edges) -> implement (build via TDD) -> code-review (two-axis review). Use when the user starts or continues a feature build, wants the structured workflow, or says 开发/实现/开始做/按流程走. Trivial changes (typo, copy tweak, one-line fix) skip the pipeline and are fixed directly."
---

# zengbohan-skill

A five-stage pipeline for feature-sized work. Walk it through the stages in order, one unbroken banner at a time, and do not treat an unpublished draft as a settled output. Quick path: pure typo fixes, copy tweaks, single-line fixes, and other micro-changes skip stages 1–3 entirely — implement directly and say at the start of the reply that you took the quick path.

This skill is self-contained: every stage definition it drives lives under `stages/` in this folder. It has no external skill dependencies — install this one folder and nothing else.

Phase-break and context rules follow the phase-boundary tree (see `stages/ask-matt/PHASE-BOUNDARIES.md`; the other slash commands mentioned in `stages/ask-matt/` are background reading from the upstream suite, not installed): keep stages 1–3 in one context window so the interview, spec, and tickets build on the same thinking; each `/implement` then starts fresh from its ticket.

## Stage 0 — Precondition check (once)

Before the first stage, confirm the issue tracker is configured for this repo:

- If `docs/agents/issue-tracker.md` exists, read it and proceed.
- If it is missing, run setup-matt-pocock-skills (read `stages/setup-matt-pocock-skills/setup-matt-pocock-skills.md`) to configure the issue tracker, triage label vocabulary, and domain doc layout. Present the full proposed configuration in one pass and confirm it with the user once before writing — not choice by choice.

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
- Sketch the seams at which the feature will be tested (prefer existing seams, highest seam possible, fewest total) and record them in the spec's Testing Decisions. The single seam confirmation is the one to-spec.md directs — this stage adds no second sign-off.
- Write the spec using the template in `stages/to-spec/to-spec.md` (Problem Statement, Solution, User Stories, Implementation Decisions, Testing Decisions, Out of Scope, Further Notes). No file paths or code snippets unless a prototype encoded a decision precisely.
- Publish it to the issue tracker with the `ready-for-agent` label (per `docs/agents/issue-tracker.md`).
- **Stage exit:** spec published with seams settled.

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
- Use TDD where it fits, at the pre-agreed seams (read `stages/tdd/tdd.md`), one red-green slice at a time; for tickets where TDD doesn't apply (pure docs/config), say so in the ticket instead of forcing tests. Run typechecking and single-test-file regularly; run the full test suite once at the end.
- Commit each completed ticket to the current branch.
- **Stage exit:** all tickets built, committed, full test suite green.

## Stage 5 — code-review

Review the whole change along two independent axes.

- Load `stages/code-review/code-review.md`.
- Pin the fixed point (branch start / merge-base / pre-feature commit), confirm it resolves and the diff is non-empty.
- Point the Standards sub-agent at any documented coding standards plus the smell baseline from `stages/code-review/code-review.md`; point the Spec sub-agent at the spec/tickets. Run them in parallel, then aggregate verbatim under `## Standards` and `## Spec`.
- Fix blocking findings and re-run the review at most once more; judgement-call smells stay recorded in the report instead of being looped on until "clean".
- **Stage exit:** both axes report no blocking findings (remaining judgement calls, if any, documented) and the final report is delivered.

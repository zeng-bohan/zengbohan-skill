---
name: zengbohan-skill
description: "Three-tier development pipeline for feature work: micro fixes go straight in; standard (the default) is a one-round interview -> one plan approval -> in-session TDD build -> review; full adds multi-round grilling, a published spec, and tracer-bullet tickets for multi-session builds. Use when the user starts or continues a feature build, wants the structured workflow, or says 开发/实现/开始做/按流程走. Trivial changes (typo, copy tweak, one-line fix) skip the pipeline and are fixed directly."
---

# zengbohan-skill

A three-tier pipeline for development work. One pipeline, two depths — every tier above micro runs the same four stages; the tier only decides how deep each stage goes. Stage rules live in `stages/`; this file picks the tier and routes.

Say in the first line of your reply which tier you took and why.

## Pick the tier

| Tier | When | What runs |
| --- | --- | --- |
| **micro** | typo, copy tweak, one-line fix, or anything smaller than a feature | fix directly, nothing else |
| **standard** (default) | the feature fits one context window and about one sitting | the four stages at standard depth |
| **full** | the feature will span sessions or days, or the user explicitly wants spec + tickets on the tracker | the four stages at full depth |

When torn between standard and full, take standard and offer to escalate once the plan is on the table. Escalating mid-flight means deepening the plan stage (publish the plan as a spec and split out tickets), not starting over.

## Shared rules

- **Facts are your job, decisions are the user's.** When a question needs an answer from the environment (filesystem, tools, docs, DB), look it up or dispatch a sub-agent — never put it to the user. Only decisions go to the user, and each carries a recommended answer.
- **Context hygiene** follows `references/PHASE-BOUNDARIES.md`: continuing the session is always the first option; clear, compact, or split only at stage boundaries.

## Stage 0 — setup (once per repo, on demand)

Only the full tier needs tracker artifacts, so setup runs lazily: the first time a stage wants `docs/agents/issue-tracker.md` and it is missing, load `stages/setup.md`. It presents the full proposed configuration in one pass and writes after a single confirmation.

## The four stages

Run them in order. Each stage file carries its own rules; the line here is its exit condition only.

1. **interview** — load `stages/interview.md`. Sharpen the idea into a settled design tree, leaving `CONTEXT.md` and ADRs behind. *Exit:* standard — the top-level decisions and their immediate consequences are settled; full — the frontier is empty and the user confirms shared understanding.
2. **plan** — load `stages/plan.md`. Synthesize (no new interview) into a plan: decisions, seams, tickets. *Exit:* standard — the user approves the one-pager (one gate covering seams, granularity, and edges in a single exchange); full — the spec (seams included) is confirmed and published, then the ticket breakdown is approved and published.
3. **implement** — load `stages/implement.md`. Build the tickets test-first at the pre-agreed seams. *Exit:* all tickets built and committed, full test suite green.
4. **code-review** — load `stages/code-review.md`. Two-axis review (Standards + Spec). *Exit:* no blocking findings on either axis; remaining judgement calls documented in the report; report delivered.

Keep interview and plan in one context window — they build on the same thinking. Implement continues in the same session by default; `stages/implement.md` says when to split.

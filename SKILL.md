---
name: zengbohan-skill
description: "Three-tier development pipeline for feature work: micro fixes go straight in; standard (the default) is a one-round interview -> one plan approval -> in-session TDD build -> review; full adds multi-round grilling and a two-gate plan for multi-session builds."
disable-model-invocation: true
---

# zengbohan-skill

A three-tier pipeline for development work, run only when explicitly invoked (`/zengbohan-skill <task>`). One pipeline, two depths — every tier above micro runs the same four stages; the tier only decides how deep each stage goes. Stage rules live in `stages/`; this file picks the tier and routes.

Say in the first line of your reply which tier you took and why.

## Pick the tier

| Tier | When | What runs |
| --- | --- | --- |
| **micro** | typo, copy tweak, one-line fix, or anything smaller than a feature | fix directly, nothing else |
| **standard** (default) | the feature fits one context window and about one sitting | the four stages at standard depth |
| **full** | the feature will span sessions or days | the four stages at full depth |

When torn between standard and full, take standard and offer to escalate once the plan is on the table. Escalating mid-flight means deepening the plan stage (a fuller interview and a finer slice breakdown), not starting over.

## Shared rules

- **Facts are your job, decisions are the user's.** When a question needs an answer from the environment (filesystem, tools, docs, DB), look it up or dispatch a sub-agent — never put it to the user. Only decisions go to the user, and each carries a recommended answer.
- **Context hygiene** follows `references/PHASE-BOUNDARIES.md`: continuing the session is always the first option; clear, compact, or split only at stage boundaries.
- **One artifact only.** The pipeline writes a single file — the plan under `.scratch/<feature-slug>/plan.md`. No glossaries, ADRs, specs, or ticket files; everything else lives in the conversation.

## The four stages

Run them in order. Each stage file carries its own rules; the line here is its exit condition only.

1. **interview** — load `stages/interview.md`. Explore unfamiliar code with parallel sub-agents, then sharpen the idea into a settled design tree held in conversation. *Exit:* standard — the top-level decisions and their immediate consequences are settled; full — the frontier is empty and the user confirms shared understanding.
2. **plan** — load `stages/plan.md`. Synthesize (no new interview) into the plan file: problem, stories, decisions, seams, slices. *Exit:* standard — the user approves the plan in one gate; full — two gates: the plan's content, then its slice breakdown.
3. **implement** — load `stages/implement.md`. Build the slices test-first at the pre-agreed seams. *Exit:* all slices built, committed and ticked off in the plan file, full test suite green.
4. **code-review** — load `stages/code-review.md`. Two-axis review (Standards + Plan), then an acceptance pass. *Exit:* no blocking findings on either axis; remaining judgement calls documented; an acceptance checklist (deliverable → evidence) handed to the user for sign-off.

Keep interview and plan in one context window — they build on the same thinking. Implement continues in the same session by default; `stages/implement.md` says when to split.

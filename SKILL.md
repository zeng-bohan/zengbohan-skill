---
name: zengbohan-skill
description: "Explicitly invoked development workflow for features, fixes, and refactors: risk-based micro/standard/full tiers, one resumable plan, verified slices, and two-axis review."
disable-model-invocation: true
---

# zengbohan-skill

Run only when the user explicitly names this skill for development work, through `$zengbohan-skill <task>`, `/zengbohan-skill <task>`, or the harness's skill picker. Generic requests to develop or follow a process do not activate it. Requests to inspect or edit the skill itself are skill maintenance, not a development pipeline invocation.

Announce the chosen tier and reason. Honour an explicit tier unless its risk requires escalation; explain the reason. Keep replies in the user's language. This entry routes; stage rules live in `stages/`.

## Pick the tier

- **micro:** local, low-risk, well-understood, easily reversible changes such as a typo or isolated copy tweak. Fix and perform proportionate verification without interview or plan. Small line count alone does not qualify.
- **standard (default):** bounded features, fixes, or refactors whose decisions and dependencies can be settled in one short planning exchange. Run four stages at standard depth.
- **full:** work spanning sessions, with multiple dependent deliverables or substantial architectural uncertainty. Run four stages at full depth.

Changes involving authorization, money, destructive operations, migrations, shared API compatibility, or concurrency/integrity are at least standard regardless of size. Prefer standard over full when uncertain; escalate by extending the existing plan, never restarting settled work.

## Shared rules

- Look up accessible environment facts. Ask the user only for missing business knowledge, unavailable information, and decisions that materially affect scope, behaviour, or risk; recommend an answer. Resolve routine engineering choices yourself.
- Preserve explicit authorization and restrictions across stages. Plan approval does not by itself authorize commits, pushes, merges, deployments, or live database writes. Follow the execution policy recorded in the plan.
- The only workflow document is `.scratch/<feature-slug>/plan.md`. Code, tests, and requested product deliverables are not workflow documents. No extra ADRs, specs, tickets, or handoff files.
- Use available tools; parallel exploration/review may use subagents when supported. Fall back to sequential work when unavailable. Context handling follows `references/PHASE-BOUNDARIES.md`.

## Four stages

1. **interview** — read `stages/interview.md`. Exit when blocking decisions are settled and remaining assumptions are explicit; full also confirms shared understanding.
2. **plan** — read `stages/plan.md`. Exit with one approved plan gate for standard; content then slices for full. Existing explicit approval counts; do not re-ask.
3. **implement** — read `stages/implement.md`. Exit when planned behaviour is implemented and agreed checks pass; commit only under the execution policy. Record incomplete checks honestly.
4. **code-review** — read `stages/code-review.md`. Exit with no blocking findings and an evidence-backed acceptance checklist. Report delivery separately from pending user sign-off.

Continue in the same session by default. Resume by reading the plan and verifying the current workspace, not by trusting checkboxes alone.

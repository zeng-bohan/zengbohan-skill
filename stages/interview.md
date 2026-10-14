# Interview

Ground the task in the existing code, then settle the decisions needed to plan it. Keep exploration and discussion in conversation until the plan stage.

## Explore before asking

Inspect relevant repository instructions, current branch/worktree changes, entry points, and similar existing behaviour. For an unfamiliar, broad area, use two or three parallel explore subagents when available: similar features, architecture, and current behaviour. For a narrow change, inspect directly. Read the key files yourself before relying on recommendations.

Find accessible facts yourself. If evidence is unavailable, identify the gap and seek only the missing information; never invent a fact to avoid asking. Distinguish intended requirements from what current code actually does.

## Questions and depth

Track decisions and their dependencies as a design tree in conversation. Ask only questions whose prerequisites are known; prioritize 3–5 material questions per round, fewer when sufficient. Give each a concise title, concrete options when useful, and a recommendation with its consequence. Do not send a long questionnaire just because many leaves remain.

- **standard:** normally one round; skip questions already answered. Use a second round only if it changes scope, behaviour, API/data contracts, risk, or the plan's shape. Carry routine choices into implementation with reasonable defaults.
- **full:** work through blocking branches, then summarize settled decisions, explicit assumptions, and remaining risks for confirmation. Stop when the plan can be executed safely; do not attempt to exhaust every conceivable branch.

Wait for answers to blocking questions. While waiting, continue independent read-only exploration. Prior explicit authorization remains valid.

## Domain precision

Use concrete success and failure scenarios, canonical domain terms, and existing contracts. Resolve material vocabulary conflicts; do not interrogate harmless wording differences. Challenge discrepancies between the user's account and observed code with evidence. Bring confirmed terms, assumptions, and acceptance conditions into the plan.

# Code review & acceptance

Review two axes independently: **Standards & correctness** and **Plan & behaviour**. Use two parallel subagents when available, otherwise separate sequential passes. Aggregate actionable findings by severity, retaining an axis label and deduplicating overlaps.

## Define the complete review scope

For this pipeline, use the starting SHA and task scope recorded in the plan; do not ask the user to supply facts already recorded. For standalone review, respect the requested scope. Resolve an unspecified base from repository/branch context when unambiguous, otherwise ask one focused question.

Validate the ref as a commit and pin its SHA. Use `git diff <base-sha> HEAD` for changes since an exact ancestor commit. For a branch/PR comparison, pin and record the merge-base and compare from there; do not silently turn an exact revision request into merge-base semantics. If the base is not an ancestor, clarify the intended comparison when needed.

Also inspect `git status --short`, staged and unstaged diffs, and task-owned untracked files when reviewing current work. Untracked contents require explicit inspection; a Git diff alone does not include them. Use the final workspace state plus relevant deltas, not overlapping diffs counted as separate changes. Keep pre-existing unrelated edits out of task findings and staging; note dependencies on them if relevant.

An empty committed diff is not an error if changes are uncommitted. If the entire requested scope is empty, report no changes to review. For non-Git work inspect the changed files and available before/after evidence, stating scope limitations.

## Identify inputs

Use the current plan, or a user-specified plan path, then a matching `.scratch/` plan. Without a plan, use explicit user requirements as the behaviour reference; state that plan coverage cannot be established if neither exists.

Read applicable repository standards and surrounding code. Lint/type tooling results are evidence, not a substitute for correctness review.

## Two passes

Give each reviewer the pinned scope, pre-existing-change exclusions, relevant files, and verification results.

1. **Standards & correctness:** inspect actual behaviour for defects, error paths, authorization, transactions, concurrency, data integrity, and compatibility as relevant to the change. Cite applicable documented standards for hard violations. Treat naming, duplication, misplaced responsibilities, unnecessary abstractions, and other code smells as optional judgement calls unless they produce a concrete defect. Repository conventions override stylistic preferences. Do not recommend broad refactoring just to remove a smell.
2. **Plan & behaviour:** map requirements and acceptance conditions to implementation. Look for missing/partial behaviour, wrong behaviour, scope creep, and requirements or assumptions contradicted by evidence. A flawed plan does not excuse a defect; surface it and use controlled change when required.

Each actionable finding needs severity, file/line, trigger, impact, evidence, and a suggested correction. Distinguish verified defects, unverified risks, and optional suggestions. Show important findings first; keep reports concise without dropping evidence to satisfy a fixed word limit.

## Fix and recheck

Fix clear blocking defects within authorized scope. Re-run affected checks and review changed areas. Limit to two review rounds by default to prevent style loops. If blocking findings remain after that, report incomplete status, evidence, and the next required action; never relax the completion criteria to fit the round limit. Optional smells do not block delivery.

## Acceptance

Map every slice/story to its observed verification evidence, result, and repeatable command or interaction. Show what passed, failed, and remains unavailable. Required deployment, real integration, or device checks must stay pending until evidenced; local success alone is not final acceptance.

Update the same plan's execution state and present an acceptance checklist. Report separately: implementation status, verification status, commit state, and user sign-off. Ask for sign-off only when required and not already given; do not claim user acceptance merely because automated review passed.

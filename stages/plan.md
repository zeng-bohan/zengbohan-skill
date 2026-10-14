# Plan

Synthesize the settled requirements and inspected code. Do not restart the interview; if a newly discovered fact invalidates a material decision, surface only that decision.

Write the draft to `.scratch/<feature-slug>/plan.md` before presenting it for approval. Update this one file through planning, implementation, review, and resumption. Keep it concise; include stable module names, APIs, and useful file paths as navigation hints, verified again on resume. Avoid speculative implementation listings.

## Plan template

### Problem & acceptance

Describe the problem and 2–4 relevant stories, or fewer for a narrow fix. Give observable success conditions and material failure cases. Distinguish local verification from deployment, integration, or device acceptance when those are required.

### Decisions & assumptions

Record scope, canonical terms, interface/schema changes, material technical choices and their reasons. List assumptions and risks still requiring verification.

### Execution policy

Record repository/worktree, branch, starting HEAD SHA when Git is available, relevant pre-existing changes, and review scope. Record the user's existing permissions and restrictions. Commit mode: leave uncommitted by default unless already authorized; otherwise per-slice or final commit as agreed. Push, merge, deployment, and live database operations require their own applicable authorization. Do not ask again for authorization already given.

### Verification

Prefer existing public boundaries. Choose the smallest set that covers behaviour and material risks, including lower-level integration checks when appropriate. Record related tests per slice, the final check set, and any unavailable environment checks. Coverage matters more than minimizing the number of seams. New routine tests within the agreed strategy need no extra approval.

### Slices

Number slices in dependency order. For each include:

- **S1 — title**; blockers: none or earlier slices; delivers: observable behaviour; verify: command/interaction and expected result; status: pending / implemented / verified; commit: SHA or uncommitted.

Prefer narrow end-to-end slices through the layers actually involved, each independently verifiable. Do not invent UI or schema work for a backend-only change. For wide mechanical refactors, use expand–migrate–contract; record any shared integration checkpoint when batches cannot remain green independently.

### Out of scope

State the boundaries needed to prevent scope creep.

### Execution state

Keep current phase, latest verified result, unresolved blockers, plan changes, next action, and acceptance status (pending / accepted) here. Add evidence and commit SHAs as work progresses. One checkbox must not imply implementation, verification, commit, and user acceptance simultaneously.

## Approval gates

- **standard:** present one concrete draft covering scope, decisions, verification, slices, and execution policy. Ask for approval once unless already explicitly approved. Update only the parts the user changes.
- **full:** first approve content and verification, then add and approve slices/dependencies in the same file. Reuse existing approval when it covers the presented decision.

Material changes after approval follow the controlled-change rules in `implement.md`; local engineering choices do not create new gates.

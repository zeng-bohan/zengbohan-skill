# Implement

Read the approved plan, repository instructions, and current workspace. Preserve pre-existing user changes. On resume verify branch, files, commits, and available check results against the execution state before taking the next slice.

## Work the slices

Build one unblocked slice at a time in this session by default. Use subagents only for independently scoped work with clear ownership and dependencies; the primary agent integrates and verifies results. Follow `../references/PHASE-BOUNDARIES.md` when context requires a transition.

## Controlled changes

- Routine implementation details: choose and proceed.
- Small adjustments that preserve agreed behaviour, contracts, scope, and risk: update the plan and inform the user; no new approval.
- Changes to business behaviour, scope, public contracts, schema, or material risk: explain the new evidence and smallest proposed plan change, then wait for approval of the affected decision. Continue unaffected work where possible.

Do not silently deviate or repeatedly reopen settled design. An infeasible or unsafe plan is a reason to amend it, not to force implementation.

## Build and verify

Use behaviour-focused TDD when useful: failing test, minimal implementation, then small behaviour-preserving refactoring while green. Reproduce a bug before fixing it when practical. For docs/config or other changes where TDD adds no value, record and perform the appropriate check instead.

Test at stable boundaries using the agreed strategy, not private implementation details. Add routine boundary tests without another gate. Reconfirm only a material strategy/interface change. Read `../references/tests.md` and `../references/mocking.md` when choosing tests or mocks.

Expected values need an independent source of truth. Prefer real integration paths for persistence, transactions, constraints, and migrations. Direct database assertions are appropriate when stored state or integrity is itself a requirement; use a test database, never infer permission to mutate a live database.

Run affected tests and relevant type/compile/integration checks during each slice. Run the final checks agreed in the plan after integration; broaden earlier for shared contracts or high-risk changes. Do not repeatedly run an expensive whole-repository suite without a reason. Record commands, results, and unavailable checks; compilation or mocked tests do not stand in for required environment acceptance.

## Record and commit

Update each slice as implemented, then verified only when its checks pass. Commit only if the plan's execution policy authorizes it, after checking the branch and inspecting staged contents; include only task-owned changes. Record the SHA or uncommitted state separately. Do not push or merge under development approval alone.

Keep the plan's execution state current at slice boundaries and before handoff. If a check fails or cannot run, record the limitation and next step; do not mark the slice verified. When all slices are ready, move to review even if authorized work is intentionally left uncommitted.

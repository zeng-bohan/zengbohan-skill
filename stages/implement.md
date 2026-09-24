---
name: implement
description: Build the planned tickets test-first at the pre-agreed seams — in-session by default, one commit per ticket. Never reopens the plan.
---

# Implement

Implement the work described by the plan or tickets. **Never reopen the plan** — no interview, no clarifying round, no proposing a different approach. Whatever was settled upstream is the input; the job is to turn it into commits.

## Where the work runs

Work the **frontier**: any ticket whose blockers are all done, in dependency order.

- **Default: this session, ticket by ticket.** Work the frontier — standard: the first unchecked ticket in the plan file whose blockers are all ticked; full: any open ticket with no open blockers. Build it, commit it, move on. Context from earlier tickets is an asset — it holds the reasoning — not clutter. The marks are also the resume point: a fresh session reads the same plan file or tracker and picks up exactly where the last one left off.
- **Split only when needed** (see `../references/PHASE-BOUNDARIES.md`): if the context approaches the smart zone mid-build, finish the current ticket, commit, then hand the next ticket to a fresh session seeded from its ticket file, or dispatch it to a sub-agent. Tickets are self-contained by design, which is what makes this safe. A ticket that can run unattended is a good sub-agent candidate.

## The build loop

Use TDD where it fits, at the pre-agreed seams (rules below). For tickets where TDD doesn't apply (pure docs/config), say so in the ticket instead of forcing tests. Run typechecking and single test files regularly; run the full test suite once at the end of each ticket. Commit each completed ticket to the current branch, **then mark it done** — tick its checkbox in the plan file (standard), close its issue (real tracker), or set `Status: done` in its ticket file (local-markdown tracker). An unmarked ticket looks unfinished to the next session; the marks are what make resume work.

## TDD — the red → green loop

TDD is the red → green loop. The rules below are what make that loop produce tests worth keeping: what a good test is, where tests go, the anti-patterns, and the rules of the loop. Every section applies on every cycle — consult them before and during the loop, not after.

When exploring the codebase, read `CONTEXT.md` (if it exists) so test names and interface vocabulary match the project's domain language, and respect ADRs in the area you're touching.

### What a good test is

Tests verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification — "user can checkout with valid cart" tells you exactly what capability exists — and survives refactors because it doesn't care about internal structure.

See `../references/tests.md` for examples and `../references/mocking.md` for mocking guidelines.

### Seams — where tests go

A **seam** is the public boundary you test at: the interface where you observe behavior without reaching inside. Tests live at seams, never against internals.

**Test only at the seams pre-agreed in the plan.** No test is written at an unconfirmed seam. You can't test everything — agreeing the seams up front is how testing effort lands on the critical paths and complex logic instead of every edge case.

If a ticket genuinely needs a new seam, propose it to the user once and wait for approval before writing tests at it.

When the shape of an interface is itself in question — how deep the module is, where the seam belongs, what it should expose — spell out the candidate shapes and their trade-offs for the user before picking one.

### Anti-patterns

- **Implementation-coupled** — mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological** — the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth — a known-good literal, a worked example, the spec.
- **Horizontal slicing** — writing all tests first, then all implementation. Bulk tests verify _imagined_ behavior: you test the _shape_ of things rather than user-facing behavior, the tests go insensitive to real changes, and you commit to test structure before understanding the implementation. Work in **vertical slices** instead — one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

### Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring is not part of the loop.** It belongs to the review stage (see `code-review.md`), not the red → green implementation cycle.

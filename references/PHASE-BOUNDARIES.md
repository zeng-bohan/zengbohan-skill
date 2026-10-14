# Context and phase boundaries

Continue in the current session when sufficient context remains. Prefer intentional transitions at completed slice/stage boundaries, but automatic compaction or interruption may occur at any time; recover from the plan and actual workspace.

Use the harness's available context information rather than a fixed token threshold. If no metric is exposed, judge by remaining work and signs that relevant decisions are being lost. Do not claim to control unsupported context commands.

Before a transition, update the existing plan with current phase, verified slices, commit/uncommitted state, unresolved decisions, evidence, and next action. A slice entry alone is insufficient when there are cross-slice dependencies.

Choose the supported option that preserves required context:

- Continue for dependent work that still fits.
- Compact when space is needed; preserve requirements, decisions, restrictions, and next action in the summary.
- Delegate independently scoped work to a subagent when available, with ownership, inputs, dependencies, and expected evidence.
- Use a fresh session only when needed; seed it with the same plan path and repository/worktree. Resume by verifying actual state.
- Clear only when the previous context is genuinely irrelevant and necessary state is preserved.

Commands such as `/compact`, `/clear`, and `/handoff` vary by harness. Use them only when supported; otherwise provide a concise continuation prompt in conversation pointing at the plan. Do not create an additional handoff document or a new user-owned chat without authorization.

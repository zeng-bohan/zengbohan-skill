---
name: setup
description: Configure this repo's issue tracker and domain doc layout — once, before the first full-tier run that needs tracker artifacts.
---

# Setup

Scaffold the per-repo configuration the pipeline's full tier assumes:

- **Issue tracker** — where issues live (GitHub by default; local markdown is also supported out of the box)
- **Triage labels** — the label strings used for the five canonical triage roles
- **Domain docs** — where `CONTEXT.md` and ADRs live, and the consumer rules for reading them

This is a prompt-driven procedure, not a deterministic script: explore, present, confirm once, then write. **Present the full proposed configuration in one pass** — every section below with its recommended answer — and take a single confirmation before writing. Do not ask section by section.

## 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` and `.git/config` — is this a GitHub repo? GitLab? Which one?
- `AGENTS.md` and `CLAUDE.md` at the repo root — does either exist? Is there already an `## Agent skills` section in either?
- `CONTEXT.md` and `CONTEXT-MAP.md` at the repo root
- `docs/adr/` and any `src/*/docs/adr/` directories
- `docs/agents/` — does this procedure's prior output already exist?
- `.scratch/` — a sign that a local-markdown issue tracker convention is already in use
- Monorepo signals — a `pnpm-workspace.yaml`, a `workspaces` field in `package.json`, or a populated `packages/*` with its own `src/`. Present only in a genuinely large multi-package repo; their absence means single-context, which is almost every repo.

## 2. Decide (recommendations, presented together)

**Issue tracker.** Default posture: propose whatever the `git remote` points at — GitHub (via `gh`), GitLab (via `glab`). Otherwise offer:

- **GitHub** — issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab** — issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Local markdown** — issues live as files under `.scratch/<feature>/` in this repo (good for solo projects or repos without a remote)
- **Other** (Jira, Linear, etc.) — ask the user to describe the workflow in one paragraph; record it as freeform prose

Record the choice in `docs/agents/issue-tracker.md`. The GitHub and GitLab templates carry a "PRs as a request surface" flag, defaulted **off** — leave it off and don't raise it; a user who wants external PRs in the triage queue can flip the flag in the file later.

**Triage labels.** Write the five defaults as-is (see `../references/triage-labels.md`): `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. No question needed — mention they can edit `docs/agents/triage-labels.md` later if the tracker already uses other names.

**Domain docs.** Default to **single-context** — one `CONTEXT.md` + `docs/adr/` at the repo root. This fits almost every repo; propose it without asking. Offer **multi-context** (a root `CONTEXT-MAP.md` pointing to per-context `CONTEXT.md` files) only when exploration found monorepo signals.

## 3. Present and confirm once

Show the user, in one message:

- The `## Agent skills` block to add to whichever of `CLAUDE.md` / `AGENTS.md` is being edited (see step 4)
- The contents of `docs/agents/issue-tracker.md`, `docs/agents/domain.md`, and `docs/agents/triage-labels.md`

Let them adjust anything in that single exchange, then write.

## 4. Write

**Pick the file to edit:**

- If `CLAUDE.md` exists, edit it.
- Else if `AGENTS.md` exists, edit it.
- If neither exists, ask the user which one to create — don't pick for them.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa) — always edit the one that's already there.

If an `## Agent skills` block already exists in the chosen file, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections.

The block:

```markdown
## Agent skills

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout — "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

Then write the docs files using the seed templates in `../references/` as a starting point:

- `../references/issue-tracker-github.md` — GitHub issue tracker
- `../references/issue-tracker-gitlab.md` — GitLab issue tracker
- `../references/issue-tracker-local.md` — local-markdown issue tracker
- `../references/triage-labels.md` — label mapping
- `../references/domain.md` — domain doc consumer rules + layout

For "other" issue trackers, write `docs/agents/issue-tracker.md` from scratch using the user's description.

## 5. Done

Tell the user the setup is complete. Mention they can edit `docs/agents/*.md` directly later — re-running this procedure is only necessary if they want to switch issue trackers or restart from scratch.

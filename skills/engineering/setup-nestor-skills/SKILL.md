---
name: setup-nestor-skills
description: Sets up an `## Agent skills` block in the project's agent instructions file (`AGENTS.md`, or `CLAUDE.md` if it already exists) and `docs/agents/` so the engineering skills know this repo's issue tracker (GitHub or local markdown), triage label vocabulary, domain doc layout, and the LLM Wiki location. Run before first use of `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out` — or if those skills appear to be missing context about the issue tracker, triage labels, or domain docs.
disable-model-invocation: true
---

# Setup Matt Pocock's Skills

Scaffold the per-repo configuration that the engineering skills assume:

- **Issue tracker** — where issues live (Beads by default; local markdown is also supported out of the box)
- **Triage labels** — the strings used for the five canonical triage roles
- **Domain docs** — where the LLM Wiki (under `knowledge-base/`) and ADRs live, and the consumer rules for reading them

This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm with the user, then write.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` and `.git/config` — is this a GitHub repo? Which one?
- `AGENTS.md` and `CLAUDE.md` at the repo root — which one exists? (If both exist, `CLAUDE.md` wins — see step 4.) Is there already an `## Agent skills` section in the file that will be edited?
- The LLM Wiki directory at the repo root (default `knowledge-base/`) — does it exist?
- `docs/adr/` and any `src/*/docs/adr/` directories
- `docs/agents/` — does this skill's prior output already exist?
- `.scratch/` — sign that a local-markdown issue tracker convention is already in use

### 2. Present findings and ask

Summarise what's present and what's missing. Then walk the user through the three decisions **one at a time** — present a section, get the user's answer, then move to the next. Don't dump all three at once.

Assume the user does not know what these terms mean. Each section starts with a short explainer (what it is, why these skills need it, what changes if they pick differently). Then show the choices and the default.

**Section A — Issue tracker.**

> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-issues`, `triage`, `to-prd`, and `qa` read from and write to it — they need to know whether to call `gh issue create`, write a markdown file under `.scratch/`, or follow some other workflow you describe. Pick the place you actually track work for this repo.

Default posture: these skills were designed for GitHub. If a `git remote` points at GitHub, propose that. If a `git remote` points at GitLab (`gitlab.com` or a self-hosted host), propose GitLab. Otherwise (or if the user prefers), offer:

- **GitHub** — issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab** — issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Local markdown** — issues live as files under `.scratch/<feature>/` in this repo (good for solo projects or repos without a remote)
- **Beads** — issues live in a local/shared Dolt database (uses the `bd` CLI)
- **Other** (Jira, Linear, etc.) — ask the user to describe the workflow in one paragraph; the skill will record it as freeform prose

**Section B — Triage label vocabulary.**

> Explainer: When the `triage` skill processes an incoming issue, it moves it through a state machine — needs evaluation, waiting on reporter, ready for an AFK agent to pick up, ready for a human, or won't fix. To do that, it needs to apply labels (or the equivalent in your issue tracker) that match strings *you've actually configured*. If your repo already uses different label names (e.g. `bug:triage` instead of `needs-triage`), map them here so the skill applies the right ones instead of creating duplicates.

The five canonical roles:

- `needs-triage` — maintainer needs to evaluate
- `needs-info` — waiting on reporter
- `ready-for-agent` — fully specified, AFK-ready (an agent can pick it up with no human context)
- `ready-for-human` — needs human implementation
- `wontfix` — will not be actioned

Default: each role's string equals its name. Ask the user if they want to override any. If their issue tracker has no existing labels, the defaults are fine.

**Section C — Domain docs.**

> Explainer: Some skills (`improve-codebase-architecture`, `diagnose`, `tdd`) read the LLM Wiki to learn the project's domain language, and `docs/adr/` for past architectural decisions. The LLM Wiki is a directory of markdown files the project maintains as its source of truth for concepts and entities.

Confirm setup — two questions, in order:

1. **LLM Wiki location.** Ask the user where the LLM Wiki lives in this repo. The default is `./knowledge-base` at the repo root (so absolute-from-repo paths like `knowledge-base/wiki/index.md`). If the user has put it somewhere else (e.g. `./docs/wiki`, `~/some-shared/team-wiki`, a path outside the repo), capture the path as they want it written into `AGENTS.md` — relative paths are interpreted from the repo root, absolute paths are taken as-is. The chosen path is stored in the config and substituted into both the `AGENTS.md` block and `docs/agents/domain.md`.
2. **ADRs.** Default to `docs/adr/` at the repo root. Ask the user only if the explore step turned up an existing `adr/` directory somewhere non-standard.

### 3. Confirm and edit

Show the user a draft of:

- The `## Agent skills` block to add to `AGENTS.md` (with the wiki path from Section C substituted into the Domain docs subsection)
- The contents of `docs/agents/issue-tracker.md`, `docs/agents/triage-labels.md`, `docs/agents/domain.md` (with the wiki path substituted into `domain.md`)

Let them edit before writing.

### 4. Write

**Edit the agent instructions file:**

- If `CLAUDE.md` exists, use it.
- Otherwise use `AGENTS.md` (create it with the `## Agent skills` block as its only content if it doesn't exist).
- Pick deterministically — do not ask the user.

Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa) — always edit the one that's already there. Never create `CLAUDE.md` in any case — it's the user's file, only edit it if it's already there.

If an `## Agent skills` block already exists in the chosen file, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections.

The block (substitute the wiki path from Section C where you see `[wiki-path]`):

```markdown
## Agent skills

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

The LLM Wiki is located under `[wiki-path]` at the repo root (e.g. `[wiki-path]/wiki/index.md`, `[wiki-path]/wiki/<Concept>.md`). See `docs/agents/domain.md`.
```

Then write the three docs files using the seed templates in this skill folder as a starting point:

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md) — local-markdown issue tracker
- [issue-tracker-beads.md](./issue-tracker-beads.md) — Beads issue tracker
- [triage-labels.md](./triage-labels.md) — label mapping
- [domain.md](./domain.md) — domain doc consumer rules + layout. **Substitute the wiki path from Section C** for every occurrence of `knowledge-base/` before writing — the seed template hardcodes the default path.

For "other" issue trackers, write `docs/agents/issue-tracker.md` from scratch using the user's description.

### 5. Done

Tell the user the setup is complete and **name the file you wrote to** (`AGENTS.md` or `CLAUDE.md`) so they know where the `## Agent skills` block lives. Mention they can edit `docs/agents/*.md` directly later — re-running this skill is only necessary if they want to switch issue trackers or restart from scratch.

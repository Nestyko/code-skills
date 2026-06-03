# Issue tracker: Beads

Issues and PRDs for this repo live in a local or shared Dolt database managed by the `bd` CLI.

## Conventions

- **Create an issue**: `bd create "<title>" -p <priority> -t <type> --design "<design_notes>"`.
  - Use issue types: `-t epic`, `-t task`, `-t feature`, or `-t bug`.
  - Use priorities: `-p 0` (high), `-p 1` (normal), `-p 2` (low).
  - Provide a design summary using `--design`.
- **Read an issue**: `bd show <id>`. This displays details like description, design notes, acceptance criteria, comments, and dependencies.
- **List issues**: 
  - Show ready/non-blocked work: `bd ready` (or `bd ready --json` for machine-readable format).
  - Show all issues: `bd list`.
- **Claim/Start an issue**: `bd update <id> --claim` to mark it in-progress.
- **Add notes/outcomes**: `bd update <id> --notes "outcomes content"` to record status or key decisions.
- **Comment on an issue**: `bd comments add <id> "comment text"`
- **Manage dependencies**: `bd dep add <child_id> <parent_id> --type blocks` (where the child issue cannot start until the parent issue is completed/closed).
- **Close**: `bd close <id> --reason "description of what was done"`
- **Push changes**: `bd dolt push` (if configured with a remote/shared server for collaboration).

## When a skill says "publish to the issue tracker"

Create a Beads issue using `bd create`.

## When a skill says "fetch the relevant ticket"

Run `bd show <id>` and parse the output.

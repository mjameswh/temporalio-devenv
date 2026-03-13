# Temporal Development Environment

This is the root of a multi-project AI-assisted development environment for Temporal
engineering work, owned by @mjameswh.

## Directory structure

- `all-in-one/` — Local copies of all project repositories, always on their `main`
  branch tracking upstream. Use for codebase search and reference. **Never commit,
  push, or switch branches here.**
- `workspaces/` — One subdirectory per task (named `YYYYMMDD-<description>`). Each
  workspace contains git worktrees of the relevant projects and an `instructions.md`
  describing the task.
- `skills/` — Reusable procedural instructions for recurring tasks (release processes,
  cross-repo workflows, etc.). Read the relevant skill files before undertaking those
  tasks.
- `knowledge/` — Reference material: architecture docs, project-specific facts, etc.
- `scripts/` — Automation scripts, invoked via mise tasks (see below).
- `projects.toml` — Metadata for public repos in `all-in-one/` (upstream branch names,
  IDE setup commands). Committed and safe to publish.
- `projects.local.toml` — Same schema; gitignored. Add private/non-public repo entries
  here so they don't appear in the public file. Merged automatically by scripts.

## Git conventions

Every repo in `all-in-one/` follows these conventions:
- Remote `origin` → canonical upstream (`github.com/temporalio/*` or `nexus-rpc/*`)
- Remote `mjameswh` → personal fork (when present)
- Local branch is always named `main`, regardless of what upstream calls it

Workspace project directories are **git worktrees** linked to the corresponding
`all-in-one/` repo. Branches created in a workspace are visible from the `all-in-one/`
repo.

## Available mise tasks

| Task | Description |
|---|---|
| `mise run sync-all` | Clone missing repos, fetch and fast-forward all `all-in-one` repos, run per-repo setup |
| `mise run sync-all -- --no-setup` | Same, skip setup commands |
| `mise run sync-all -- --no-clone` | Same, skip cloning of missing repos |
| `mise run add-repo <org/repo> <local-name>` | Clone a new repo into `all-in-one/` (public) |
| `mise run add-repo <org/repo> <local-name> -- --private` | Same, writing metadata to `projects.local.toml` |
| `mise run add-worktree <project> [workspace]` | Add a project worktree to a workspace |
| `mise run remove-worktree <project> [workspace]` | Remove a project worktree |
| `mise run delete-workspace <workspace>` | Delete a workspace, removing all worktrees and branches |

`add-worktree` and `remove-worktree` can also be run directly as scripts from within
a workspace directory.

## Working in a workspace

When starting a session inside a workspace, **always read `instructions.md` first**.
It contains the task description and may include an evolving execution plan. Update it
as work progresses (completed steps, decisions made, remaining work).

## Working at the devenv root

At this level, tasks typically involve environment maintenance: syncing repos, adding
new projects, creating workspaces. The `instructions.md` at this level describes the
current environment setup task.

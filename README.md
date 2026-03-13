# Temporal Internal Development Environment

An opinionated, AI-friendly workspace for working across the many repositories that
make up Temporal (SDKs, server, UI, docs, etc.). This is a work in progress at an
early stage — expect rough edges and evolving conventions.

## The problem

Day-to-day Temporal engineering work spans 30+ distinct git repositories. Most tasks
touch only one or a few of them, but searching across the full codebase, setting up
consistent tooling, and running multiple AI agents in parallel on isolated copies of
the code all require some structure beyond "clone each repo into its own directory."

## How it works

The environment is organized around two main directories:

- **`all-in-one/`** — A local clone of every project, always tracking the upstream
  main branch. This is the reference copy: use it for searching, reading, and
  cross-project navigation. Never commit or push directly here.

- **`workspaces/`** — One subdirectory per task, named `YYYYMMDD-<description>`. Each
  workspace contains git worktrees of only the projects relevant to that task, plus an
  `instructions.md` describing what needs to be done. Open a workspace in your IDE to
  work on a task.

Because workspace project directories are **git worktrees** linked to `all-in-one/`,
branches created in a workspace are visible from the corresponding `all-in-one/` repo
and vice versa — no redundant clones, no sync headaches.

Additional directories at the root (`knowledge/`, `skills/`, `scripts/`) hold
reference material, reusable procedural instructions, and automation.

## Prerequisites

- **[mise](https://mise.jdx.dev/)** — Used as a tool version manager, task runner, and
  environment variable manager. Install it first:
  - macOS (brew): `brew install mise`
  - Linux or macOS (curl): `curl https://mise.run | sh`
  - Windows: `winget install jdx.mise`
  - See https://mise.jdx.dev/getting-started.html for other options.

## Getting started

Pick a short, convenient path for the environment root (e.g. `~/devenv/temporal/`).

```sh
git clone https://github.com/mjameswh/temporalio-devenv.git my-temporal-devenv
cd my-temporal-devenv

# Trust the repo so mise will use its config
mise trust

# Install all required tool versions (Node, Python, .NET, Rust, Java, Ruby, etc.)
mise install

# Clone all repositories into all-in-one/ and run their setup commands
mise run sync-all
```

The initial sync takes a few minutes — it clones ~50 repos and installs their dependencies.
Subsequent runs only fetch and fast-forward.

## Common tasks

| Command                                      | Description                                          |
| -------------------------------------------- | ---------------------------------------------------- |
| `mise sync-all`                              | Fetch and fast-forward all repos, run setup commands |
| `mise sync-all -- --no-setup`                | Same, but skip setup commands (faster)               |
| `mise create-workspace <name>`               | Create a new task workspace                          |
| `mise add-worktree <project> [workspace]`    | Add a project to a workspace                         |
| `mise remove-worktree <project> [workspace]` | Remove a project from a workspace                    |
| `mise delete-workspace <workspace>`          | Delete a workspace and its worktrees                 |
| `mise add-repo <org/repo> <local-name>`      | Add a new repo to `all-in-one/`                      |

## Adding private repositories

Repository metadata lives in `projects.toml` (committed, public). For private or
non-public repos, add entries to `projects.local.toml` (gitignored) using the same
format. Local entries are merged automatically and take precedence.

## Working in a workspace

1. Create a workspace and open in cursor: `mise create-workspace fix-java-serialization`
2. Optionally add appropriate worktrees to the workspace: `mise add-worktree temporal-sdk-java`
3. Edit the `instructions.md` file to describe the task at hand.
4. Tell some agent (Claude, Cursor, etc.) to read the `instructions.md` file and discuss plan together.
5. The agent knows that it can search into the `all-in-one` directory to find other relevant information about the code base,
   and that it can use the `mise add-worktree` command to add other projects to the workspace as needed.
6. When done, clean up: `mise delete-workspace 20260313-fix-java-serialization`

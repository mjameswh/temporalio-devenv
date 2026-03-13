# {{WORKSPACE_NAME}}

## Starting every session

Read `instructions.md` first. It contains the task description and execution plan.
Update it as work progresses: check off completed steps, record decisions made,
and detail upcoming steps as they become clear.

## Workspace layout

Each subdirectory is a git worktree linked to the corresponding repo in
`all-in-one/` at the devenv root.

To add another project to this workspace:

```
mise run add-worktree <project>
```

You may navigate and search through the content of subdirectories under the `~/devenv/temporal/all-in-one` directory to efficiently find various facts about the full Temporal codebase. Anything under the `all-in-one` directory is readonly. The full codebase is quite vast. When possible try to only search on subsets of directories in `all-in-one`. Either list the content of `all-in-one` to get a sense of the available directories, or use the following cheatsheet (non-exhaustive):

- `temporal-sdk-<language>`: Temporal SDK for a given language, where language is one of the following: `go`, `java`, `typescript`, `python`, `dotnet`, `ruby`, `core` (used both for our "rust sdk" and as the underlying core library for our typescript, python, dotnet and ruby SDKs), `php` (officially supported but not on par with our other SDKs).
- `temporal-sdk-community-<language>`; Temporal SDKs that are developped by the community, and not officially supported by Temporal. You should rarely have use cases to examine those.
- `temporal-sdk-<language>-samples`: Usage samples for the corresponding SDK. `core` doesn't yet have a sample repository.
- `temporal/api`: the Protobuf definitions for the Temporal API (i.e. the communicaton protocol used between the server and the SDKs).
- `temporal/server`: the Temporal Server code.
- `nexus/sdk-<language>`: the Nexus SDK for a given language, where language is one of the following: `go`, `java`, `typescript`, `python`, `dotnet`.

For faster searches, you may use ripgrep (`rg`) instead of `grep`.

## Environment conventions

This workspace is part of the Temporal development environment at `~/devenv/temporal/`.
See the root `CLAUDE.md` for:

- Git remote and branch naming conventions
- Full list of available mise tasks
- Location of `skills/` and `knowledge/` reference material

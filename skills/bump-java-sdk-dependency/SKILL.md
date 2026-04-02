---
name: bump-java-sdk-dependency
description: Bump Temporal Java SDK dependency across downstream repos (samples-java, omes, features) after a new SDK release.
allowed-tools: Read, Edit, Grep, Bash(git *), Bash(gh pr *)
---

# Bump Temporal Java SDK dependency across downstream repos

Use this when a new Temporal Java SDK release (e.g. X.Y.Z) has been published and the following repos need their dependency updated, committed, pushed, and PRs opened.

## Prerequisites

- Worktrees for the three repos are already present in the workspace (e.g. created with `mise add-worktree temporal-sdk-java-samples`, `mise add-worktree omes`, `mise add-worktree features`).
- Each worktree is on the branch you will use for the PR (e.g. `java-1.33.0-update-others` or a similar branch name). No need to create new branches.
- `gh` CLI installed and authenticated. Git configured with push access to `origin` (temporalio/\* on GitHub).

## Repos and where to change the version

| Repo (worktree dir)         | Upstream GitHub repo    | File(s) to edit             | What to change                                    |
| --------------------------- | ----------------------- | --------------------------- | ------------------------------------------------- |
| `temporal-sdk-java-samples` | temporalio/samples-java | `build.gradle` (root)       | In `ext { ... }`: set `javaSDKVersion = 'X.Y.Z'`  |
| `omes`                      | temporalio/omes         | `workers/java/build.gradle` | `implementation 'io.temporal:temporal-sdk:X.Y.Z'` |

Replace `X.Y.Z` with the new SDK version (e.g. `1.33.0`).

As of April 2026, we no longer need to bump the `features` repo on SDK releases.

## Steps (per repo)

For each of the three worktrees, using the **new version number** (e.g. `1.33.0`):

1. **Update dependency**
   Edit the file(s) in the table above and set the Java SDK version to the new release (e.g. `1.33.0`).

2. **Commit and push**
   From the repo's worktree directory:
   - `git add <changed files>`
   - `git commit -m "Bump Java SDK dependency to X.Y.Z"`
   - `git push origin <current-branch>`
     (Use the actual branch name, e.g. `java-1.33.0-update-others`.)

3. **Open PR**
   From the same directory:
   - `gh pr create --base main --title "Bump Java SDK dependency to X.Y.Z" --body "Bump Java SDK dependency to X.Y.Z"`

PRs target the **main** branch of the corresponding upstream (temporalio/samples-java, temporalio/omes, temporalio/features). One PR per repo.

## Output to provide

After finishing all three repos, list the three PR URLs, for example:

- samples-java: https://github.com/temporalio/samples-java/pull/NNN
- omes: https://github.com/temporalio/omes/pull/NNN
- features: https://github.com/temporalio/features/pull/NNN

## Notes

- Worktrees may store git metadata in the main repo (e.g. `.git/worktrees/...` outside the workspace). If `git add`/`commit` fail with "Operation not permitted" on a path outside the workspace, run with permissions that allow writing there (e.g. full sandbox disable / "all").
- Only the **Java** SDK is updated; leave Python/Go/TS dependency versions unchanged unless explicitly requested.

---
name: review-pr
description: Review a GitHub pull request. 
allowed-tools: Read(/all-in-one), Grep, Bash(gh pr *)

Help me review a GitHub pull request. Use when I ask you to review a pull request.
---

## Your task

Reviewing a Pull Request (PR) in this devenv involves two distinct agents:

- A first agent, running at the root of the devenv, is responsible for setting up the workspace and providing a starting point for the next agent.
- A second agent, running inside the workspace, is responsible for providing .

Your task varies depending on whether you are responsible for the root devenv.

### If you are presently at the root of the devenv

DO NOT ATTEMPT TO REVIEW THE PULL REQUEST YOURSELF.
YOUR ONLY RESPONSIBILITY IS TO SET UP THE WORKSPACE AND PROVIDE A STARTING POINT FOR THE NEXT AGENT.

1. Use arguments I provide to determine the github repo(s) (owner/repo) and the pull request number(s).
2. Confirm that I don't already have a local workspace for this pull request. If I do, report that and provide me a clikeable link to the workspace, then stop.
3. Using the read-only clone of the target repository under the `all-in-one` directory, fetch minimal details about the pull request from GitHub, just enough to be able to synthesize the workspace name (`gh pr view <pr-number>` should be sufficient).
4. Create a new workspace in this devenv (`mise run create-workspace -- --editor <workspace-name>`), where `<workspace-name>` is a short kebab-case name, similar to `review-temporal-sdk-java-pr1234-await-cancel-timer`. Do not add a date prefix, the script will prepend the date automatically.
5. Add the appropriate worktrees to the workspace (`cd <workspace> && mise run add-worktree <project>`). To find the correct local project name for a GitHub repo, check `projects.toml` (and `projects.local.toml` if present) — the section headers are the local names used in `all-in-one/`.
6. Using the read-only clone of the target repository under the `all-in-one` directory, fetch minimal details about the pull request from GitHub (`gh pr view <pr-number>`, `gh pr view --comments <pr-number>`, `gh pr diff <pr-number> --name-only`, etc.).
7. Write a short description of the present task to the `instructions.md` file in the workspace so that another agent can pick up where you left off. You are not reponsible for completing the task, you are only responsible for setting up the workspace and providing a starting point for the next agent.

YOU ARE DONE. DO NOT ATTEMPT TO REVIEW THE PULL REQUEST YOURSELF,
AND DO NOT ATTEMPT TO RUN THE CHILD AGENT YOURSELF. JUST STOP.

### If you are presently inside the workspace containing the PR to be reviewed

1. Use the `gh` CLI to fetch details about the pull request from GitHub (`gh pr view <pr-number>`, `gh pr view --comments <pr-number>`, `gh pr diff <pr-number> --name-only`, etc.).
2. Make sure the workspace is built as required to facilitate navigation and searching through the code base.
3. Make a first assessment of the pull request. Write your observations and concerns to a `review.md` file in the workspace.

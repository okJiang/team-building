# Task Management

This document defines the default task management rules for the Agent team.

## 1. Scope

- This document is the team's default rule set.
- If a repository's `AGENTS.md` defines an exception, follow that repository's `AGENTS.md`.

## 2. Core Principles

- Every task must be in one of four stages: `To Do`, `In Progress`, `In Review`, or `Done`.
- Every task must first be classified as either a development task or a non-development task.
- Every task must clearly define an `Owner` and a `Peer`.
- The `Owner` is the Agent who accepts the task and is responsible for moving it forward.
- The `Peer` is the teammate responsible for entry approval and review approval.
- The `Owner` and the `Peer` must not be the same Agent.

## 3. Definition of a Development Task

- A `development task` is any task that changes any file inside a Git-controlled repository.
- If a task modifies any tracked file, or adds, deletes, renames, or restores any file inside a Git-controlled repository, it is a development task.
- This definition includes code, documentation, configuration, scripts, tests, assets, and any other repository content.
- A `non-development task` is a task that does not change files inside a Git-controlled repository, such as analysis, discussion, or information gathering.
- Unless a repository defines an exception, a development task must follow the repository's defined development flow.
- Unless a repository defines an exception, a development task means a task that must be merged through a GitHub PR.

## 4. Task Stages

| Stage | Meaning | Minimum requirement for development tasks |
| --- | --- | --- |
| `To Do` | The task has been accepted but execution has not started. | If development is required, create an Issue first and wait for Peer approval. |
| `In Progress` | Work on the task has started. | The task may enter this stage only after the related Issue has received Peer approval. |
| `In Review` | The result has been submitted and is waiting for review. | The task must already be in the repository's defined review step. |
| `Done` | The task is finished. | The repository's completion condition must be met, and the task must be explicitly moved to Done by the Owner. |

## 5. Task Creation and Entry Criteria

- After receiving a task, the Agent must first determine whether it is a development task.
- If the task is too large, the Agent who accepts it must split it before execution begins.
- If the task is a development task, a corresponding GitHub Issue must be created in the target repository after the task is accepted.
- Every development task must have at least one Issue. Split development subtasks must each have their own Issue.
- Before development begins, the Agent must receive approval from the Peer.
- Before approval, the only allowed actions for a development task are understanding the request, gathering context, splitting the task, and creating or refining the Issue.
- Development must not start before approval.

## 6. Workspace Rules for Development Tasks

- When performing a development task, each Agent must work in their own workspace.
- For each development task, the Agent must clone the target repository under their own workspace and perform the work in that clone.
- Multiple Agents must not perform development work in the same local repository directory.
- A shared repository directory may be used for reading, discussion, or reference, but not as the execution location for development work.

## 7. Valid Approval Conditions

- Because multiple Agents share the same GitHub account, the GitHub account itself cannot be used to identify the actor.
- Every approval must explicitly contain the word `Approve`.
- `Approve` on an Issue means development may start.
- `Approve` in the review step means the task may move to the repository's next defined step, such as Merge or direct completion.
- Any approval without the explicit word `Approve`, or without a signature, is invalid.

## 8. Signature Rules

- Every Agent must clearly know who their current `Peer` is.
- If the `Peer` is not clear, development and review must not begin.
- Every Agent must use a unique and stable name as their signature.
- Every GitHub write action must be signed, including but not limited to:
  - creating an Issue
  - commenting on an Issue
  - creating a PR
  - commenting in a PR
  - reviewing a PR
  - merging a PR
- The standard signature format is a standalone final line: `Agent: <UniqueName>`
- If a GitHub action does not provide a direct place for a body, the Agent must add a nearby related comment with the signature and clearly state which action it refers to.

## 9. Default Development and Review Flow

- Unless a repository defines an exception, a development task may enter `In Progress` only after both conditions below are satisfied:
  - the related GitHub Issue has been created
  - the Peer has explicitly written `Approve` with a signature in that Issue
- Unless a repository defines an exception, a PR must be created when development is complete.
- By default, a task enters `In Review` only after a PR has been submitted.
- By default, PR review must happen in the repository's GitHub PR.
- By default, a PR must receive Peer review and an explicit signed `Approve` in a PR comment or review before it may be merged.
- By default, a PR must not be merged before it receives a valid signed `Approve` from the Peer.

## 10. Default Completion Conditions

- Unless a repository defines an exception, a task does not end automatically after a PR is merged.
- By default, after the PR is merged, the `Owner` must explicitly move the task to `Done`.
- A task is finished only after it has been explicitly moved to `Done`.

## 11. Large Task Splitting

- If a task is too large, too broad, or cannot be reviewed clearly in one pass, the Agent who accepts it must split it.
- Every split subtask must be re-evaluated to determine whether it is a development task.
- Every split development subtask must independently follow the same rule set.

## 12. Default Minimum Checklist

1. Define the current task's `Owner` and `Peer`.
2. Decide whether the task is a development task.
3. Split the task first if it is too large.
4. If it is a development task, create the corresponding Issue.
5. Get a signed `Approve` from the Peer in the Issue.
6. If it is a development task, clone the target repository into the Agent's own workspace and use that clone for development.
7. Move the task to `In Progress` and start development.
8. Enter the review step according to the current repository's rules.
9. Get a signed `Approve` from the Peer during the review step.
10. Complete merge or submission according to the current repository's rules.
11. Have the `Owner` explicitly move the task to `Done`.

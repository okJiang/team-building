# Learning and Collaboration

This document defines the default rules for continuous learning, knowledge sharing, and mutual support across the Agent team.

## 1. Scope

- This document is the team's default rule set.
- If a repository's `AGENTS.md` defines an exception, follow that repository's `AGENTS.md`.

## 2. Core Principles

- Every Agent should continuously learn from work, share useful lessons, and help the team improve.
- A mistake, process problem, or task-delivery failure should produce a learning record, not just a local fix.
- Asking other Agents for help is part of responsible execution, not a sign of failure.
- Agents should help each other grow through explanation, discussion, review, and shared practice.

## 3. Knowledge Capture

- When an Agent encounters a problem in process, execution, judgment, coordination, or task progression, the Agent should summarize the problem and the lesson learned.
- This applies to mistakes, near misses, repeated confusion, avoidable rework, blocked progress, and any issue that is likely to happen again.
- A learning record should be specific enough to help another Agent avoid the same problem.
- Team learning must not exist only on one Agent's local machine.
- Team learning is a long-term team asset and should be stored in the team's own GitHub repository.
- Each team should create and maintain its own repository under the `okJiang` user.
- The repository name should be `XXXXTeam`, where `XXXX` is the team's name.
- Learning records should be committed to that team repository so they are preserved and shared as team knowledge.
- Because the team learning repository is also a GitHub repository, changes to that repository should follow Task Management as development work.
- A useful learning record should state:
  - what happened
  - why it happened
  - what impact it caused
  - how to avoid the same problem next time
  - whether any rule, checklist, or workflow should change

## 4. Team Repository Template

- Each team should maintain one dedicated GitHub repository for shared learning.
- The GitHub owner should be `okJiang`.
- The repository name template should be `XXXXTeam`, where `XXXX` is the team's name.
- The repository URL template should be `https://github.com/okJiang/XXXXTeam`.
- Each Agent should clone that repository into their own workspace before adding or updating team learning.
- The local clone path template should be `<agent-workspace>/okJiang/XXXXTeam`.
- A recommended initial repository structure is:

```text
XXXXTeam/
  README.md
  AGENTS.md
  docs/
    learnings/
    rules/
    templates/
      learning-record.md
```

- `README.md` should explain the team's purpose and how to navigate the repository.
- `AGENTS.md` should define repository-specific rules for that team.
- `docs/learnings/` should store durable learning records.
- `docs/rules/` should store team-specific rules if the team needs them.
- `docs/templates/learning-record.md` should provide a reusable template for new learning records.

## 5. Learning Record Template

- Unless a team repository defines another format, a new learning record should use the path template `docs/learnings/YYYY-MM-DD-<short-topic>.md`.
- A recommended learning record should include these sections:
  - `# Title`
  - `## Summary`
  - `## What Happened`
  - `## Why It Happened`
  - `## Impact`
  - `## Prevention`
  - `## Rule or Workflow Changes`
  - `## Related Tasks`

## 6. Sharing and Teaching

- If a learning is likely to help other Agents, the Agent who found it should proactively share it.
- Sharing may happen through the repository's normal collaboration channels, as long as the lesson is communicated clearly enough for others to use it.
- If the learning affects team practice, the Agent should make sure it is not only shared temporarily, but also preserved in the team's repository.
- Agents should explain not only the result, but also the reason, the pattern, and the prevention method.
- Important or repeated learnings should be turned into clearer rules, checklists, or examples when appropriate.

## 7. Mutual Support

- When an Agent is blocked, uncertain, or repeatedly failing to move a task forward, the Agent should proactively ask other Agents for help.
- Agents should not wait until the task is already badly delayed before asking for support.
- Help may include clarification, review, debugging help, alternative approaches, risk checks, or examples from past work.
- When asked for help, other Agents should respond with concrete assistance when possible.
- Agents should actively exchange useful practices and lessons so the team improves together.

## 8. Default Minimum Checklist

1. Notice the problem, mistake, or repeated difficulty.
2. Ask other Agents for help if progress is blocked or uncertain.
3. Summarize the lesson clearly enough for another Agent to reuse.
4. Preserve the learning in the team's `XXXXTeam` repository so it becomes a durable team asset.
5. Use the repository and path templates so the learning is stored in a predictable place.
6. Share the lesson with other Agents if it is likely to prevent repeated mistakes.
7. Turn the lesson into a stronger rule, checklist, or workflow when needed.

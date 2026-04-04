# Engineering Quality and Delivery

This document defines the default principles for engineering quality, continuous integration, and delivery automation across the Agent team.

## 1. Scope

- This document is the team's default rule set.
- If a repository's `AGENTS.md` defines an exception, follow that repository's `AGENTS.md`.
- This document defines principles, not repository-specific implementation details.

## 2. Core Principles

- Engineering quality should be protected by systems, not only by intention.
- If a strong development principle can be checked reliably by automation, the team should prefer enforcing it through CI.
- CI is a long-term engineering capability, not a one-time setup task.
- Teams should continuously improve their CI and delivery systems as their codebase, risks, and standards evolve.
- The goal of CI and delivery automation is sustainable quality, maintainability, and reliable team execution.

## 3. Continuous Integration Principles

- Teams should integrate automated quality checks into CI whenever those checks can be made stable and repeatable.
- GitHub Actions should be the default CI system unless a repository defines another system.
- Tests should be part of CI, not only something that runs locally by convention.
- Teams should gradually move important repeatable checks from human memory into automated CI enforcement.
- CI should be used to protect code quality, regression safety, and long-term maintainability.
- When a rule can be enforced automatically with acceptable reliability, the team should prefer automatic enforcement over manual reminders.

## 4. Testing Principles

- Code testing is a core part of engineering quality and should be integrated into CI.
- Tests in CI should help prevent regressions, protect expected behavior, and support sustainable development.
- Teams should treat missing automated test coverage as an engineering quality gap when testing is practical and valuable.

## 5. Delivery Principles

- If a project involves backend deployment, the team should prefer building and maintaining its own CD capabilities when practical.
- Delivery should become more automated as the system grows in importance, frequency of change, and operational risk.
- Deployment quality should be treated as part of engineering quality, not as a separate concern.
- Teams should reduce manual deployment risk whenever the deployment process can be made safer through automation.

## 6. Team Responsibility

- CI and delivery quality are shared team responsibilities.
- Building CI is not a one-person cleanup task. It is an ongoing team effort.
- Team members should continuously identify which quality checks, tests, and delivery safeguards should be promoted into automation.
- When the team discovers a repeated failure that could have been prevented by CI or delivery automation, that gap should be treated as a signal to improve the system.

# Phase 1: Technical Discovery

## Purpose

Build an evidence-based technical context and verification inventory without implementing application code.

## Inputs

- `.agent/intake.md`
- `.agent/open-questions.md`
- Repository contents and accessible related repositories.

## Required Context

- Repository instructions such as `AGENTS.md`, contribution rules, and scoped instruction files.
- Only repository regions relevant to the intake; retrieve additional files when evidence identifies a need.

## Procedure

1. Read repository instructions before interpreting or changing project files.
2. Map repositories, modules, services, packages, generated code, and ownership boundaries affected by the intake.
3. Identify architecture, control flow, public contracts, integration points, and implementation conventions.
4. Locate relevant source paths and similar existing implementations that should be reused.
5. Inventory tests, builds, linting, formatting, code generation, static analysis, and local environment commands.
6. Inspect schemas, migrations, persistence rules, rollback conventions, and data constraints when relevant.
7. Identify external systems, APIs, credentials, infrastructure, and repository boundaries.
8. Record technical constraints, compatibility concerns, security surfaces, operational risks, and known baseline failures.
9. Run safe read-only or baseline commands needed to confirm the verification inventory. Record commands and actual results.
10. Answer repository-discoverable open questions and update their status.
11. Write `.agent/repository-map.json` as valid JSON containing affected repositories/components, paths, relationships, external boundaries, and verification commands.
12. Write `.agent/context.md` from the canonical context template.

Do not implement, refactor, generate application code, mutate schemas, or apply infrastructure in this phase.

## Outputs

- `.agent/context.md`
- `.agent/repository-map.json`
- Updated `.agent/open-questions.md` when discovery resolves or exposes questions.

## Completion Conditions

- Affected components, paths, conventions, integration points, schemas, external systems, constraints, and risks are evidenced.
- Focused and repository-level verification commands are identified with prerequisites.
- Any baseline failure that could obstruct trustworthy verification is recorded.

## Blocking Conditions

- Required repository or specification access is unavailable.
- Repository instructions conflict.
- Architecture cannot be determined without a decision reserved for the user or another owner.
- A blocking baseline failure makes later verification unreliable.

## Transition

Proceed only to Phase 2: Specification and Decisions. Return to intake if discovery changes the understood objective or scope.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Context Management](../references/context-management.md)
- [Failure Handling](../references/failure-handling.md) for blocking baseline failures.

## Required Templates

- [Context](../templates/context.md)
- [Open Questions](../templates/open-questions.md) when updating questions.

# Phase 2: Specification and Decisions

## Purpose

Combine user intent and technical evidence into an accepted, implementable, and testable specification.

## Inputs

- `.agent/intake.md`
- `.agent/context.md`
- `.agent/repository-map.json`
- `.agent/open-questions.md`
- Relevant accepted external specifications.

## Required Context

- Intake outcomes and definition of done.
- Discovery evidence relevant to behavior and constraints.
- Existing public interfaces, security rules, schemas, and compatibility commitments affected by the work.

## Procedure

1. Specify functional behavior and observable outcomes.
2. Specify validation, failure modes, error behavior, recovery, and partial-success behavior.
3. Define actors, authorization, trust boundaries, security expectations, and sensitive-data handling.
4. Define data creation, reads, updates, retention, deletion, consistency, and lifecycle.
5. State backward/forward compatibility and public-interface requirements.
6. Record non-functional constraints such as latency, throughput, concurrency, availability, accessibility, or portability when relevant.
7. Define logging, metrics, tracing, auditing, and alerting expectations when relevant.
8. Define migration, rollout, rollback, and coexistence behavior when relevant.
9. Convert the definition of done into deterministic acceptance criteria with observable evidence.
10. Preserve explicit non-goals and identify tempting adjacent work that remains out of scope.
11. Record assumptions and classify every unresolved question as blocking, non-blocking, assumption-allowed, or resolved.
12. Create a decision record for every material contract or architecture choice that constrains another phase or node.
13. Reconcile specification language with repository behavior; never silently override either.

Do not create the implementation graph while a blocking question could materially change public behavior, architecture, storage, security, compatibility, ownership, or graph structure.

## Outputs

- `.agent/spec.md`
- `.agent/decisions.md`
- `.agent/open-questions.md`

## Completion Conditions

- Functional, error, authorization, security, data, compatibility, non-functional, observability, migration, non-goal, assumption, and acceptance sections are complete or explicitly not applicable.
- Material decisions are accepted and affected scopes are named.
- No question required before graph planning remains unresolved.

## Blocking Conditions

- Intake and repository evidence contradict each other.
- A material decision lacks an authorized owner.
- Requirements permit multiple materially different public, security, storage, compatibility, or ownership outcomes.
- Acceptance criteria cannot be made deterministic.

## Transition

Proceed only to Phase 3: Graph Planning. Reopen intake or discovery when the specification exposes missing objective or technical evidence.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Context Management](../references/context-management.md)
- [Approval Boundaries](../references/approval-boundaries.md)

## Required Templates

- [Specification](../templates/specification.md)
- [Decision Record](../templates/decision-record.md)
- [Open Questions](../templates/open-questions.md)

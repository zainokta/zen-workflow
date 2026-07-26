# Phase 0: Intake and Interview

## Purpose

Turn the user objective into a bounded problem statement without asking for facts that repository discovery can answer.

## Inputs

- User objective, supplied requirements, examples, and constraints.
- Existing accepted intake artifact when resuming.

## Required Context

- The root skill's global invariants and source-of-truth hierarchy.
- User-provided specifications only; do not load repository content merely to prolong the interview.

## Procedure

1. Record the objective and the problem in the user's terms.
2. Interview incrementally: ask one decision-relevant question at a time, then update the artifacts.
3. Establish current and expected observable behavior.
4. Identify actors, trust boundaries, authorization expectations, and affected users or systems.
5. Bound in-scope outcomes and explicit non-goals.
6. Record compatibility requirements, user-known constraints, deadlines, and operational restrictions.
7. Define done as observable, testable outcomes rather than implementation activities.
8. Separate supplied facts from assumptions.
9. Add unresolved questions to `.agent/open-questions.md` and classify each.
10. Decide whether each missing answer is:
    - blocking now;
    - required before a later phase;
    - non-blocking;
    - safe to carry as an explicit assumption.
11. Ask the user only when the answer cannot be discovered reliably and a wrong assumption could materially alter behavior, scope, security, compatibility, ownership, or architecture.

Do not ask the user to identify source files, test commands, schemas, existing patterns, or repository conventions that discovery can determine.

## Outputs

- `.agent/intake.md`
- `.agent/open-questions.md`

## Completion Conditions

- Objective, problem, current behavior, expected behavior, actors, scope, non-goals, constraints, definition of done, and assumptions are recorded.
- Every open question has an owner, classification, impact, and required phase.
- No question required before discovery remains unclassified.

## Blocking Conditions

- The intended outcome is contradictory or cannot be distinguished from its non-goals.
- A trust, authorization, or safety decision must be made before repository access.
- Required user authority or access for discovery is absent.

Persist the blocker; do not guess.

## Transition

Proceed only to Phase 1: Technical Discovery. New user evidence may reopen this phase.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Approval Boundaries](../references/approval-boundaries.md) when intake mentions consequential actions.

## Required Templates

- [Intake](../templates/intake.md)
- [Open Questions](../templates/open-questions.md)

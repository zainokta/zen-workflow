# Phase 7: Delivery

## Purpose

Prepare or perform controlled delivery work without exceeding an explicit preparation request or action-specific human authorization.

## Inputs

- Explicit request identifying delivery preparation, or explicit authorization identifying the permitted action and target.
- `.agent/reports/final-summary.md`
- `.agent/reports/review.json`
- `.agent/reports/verification.md`
- Reviewed integration commit or patch.

## Required Context

- For preparation: exact requested artifacts, candidate environment/branch/repository, scope, and constraints.
- For performance: exact authorized action, target environment/branch/repository, scope, and constraints.
- Release, migration, rollback, observability, and post-action verification requirements relevant to that action.

## Procedure

1. Classify the request as preparation or performance:
   - preparation: draft merge/release notes, deployment plan, migration plan, rollback plan, verification checklist, or observability plan;
   - performance: merge, deploy, apply migration, mutate infrastructure/data, change credentials/permissions, or run another external action.
2. Preparation requires an explicit preparation request but not authorization to execute the described action; artifacts must say `not executed`.
3. Before performance, confirm explicit authorization covers the exact action, target, environment, destructive/irreversible effects, and required credentials.
4. Reconfirm the reviewed commit/patch and required pre-action checks.
5. Prepare release notes and ordered action, migration, rollback, post-verification, and observability steps as applicable.
6. Perform only the authorized action; do not infer permission for adjacent environments, migrations, infrastructure, data, credential, permission, merge, or deployment operations.
7. Record commands/actions, actor/tool, target, timestamps, results, and resulting version/commit/state.
8. Run authorized post-delivery checks and observe relevant health, logs, metrics, traces, or alerts.
9. Stop and use the rollback/escalation plan when a safety threshold or verification gate fails; do not improvise an unapproved destructive recovery.
10. Update final state without describing prepared actions as executed.

## Outputs

- Action-specific delivery or preparation plan.
- Release notes when requested.
- Migration and rollback plan when relevant.
- Post-delivery verification and observability evidence when an action was performed.
- Updated `.agent/state.json` and final summary.

## Completion Conditions

- Preparation-only work is clearly labeled and delivery remains pending.
- Performed actions exactly match authorization and have recorded results.
- Required post-action verification passes or a failure/rollback report is persisted.

## Blocking Conditions

- For preparation, the requested artifact or scope is absent or ambiguous.
- For performance, authorization is absent, ambiguous, stale, or does not cover the exact target/action.
- Reviewed commit/patch differs from the delivery candidate.
- Required credentials, rollback capability, maintenance window, or safety check is unavailable.
- A destructive or irreversible consequence was not explicitly approved.

## Transition

No later phase. End as `completed`, `awaiting_approval`, `blocked`, or `failed` according to actual preparation/action state.

## Required References

- [Approval Boundaries](../references/approval-boundaries.md)
- [Failure Handling](../references/failure-handling.md)
- [Workflow State](../references/workflow-state.md)

## Required Templates

- [Final Report](../templates/final-report.md)

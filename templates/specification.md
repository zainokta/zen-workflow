# Engineering Specification

## Objective

{{OBJECTIVE}}

## Functional Requirements

1. {{OBSERVABLE_FUNCTIONAL_REQUIREMENT}}

## Error and Failure Behavior

- {{CONDITION}} -> {{EXPECTED_ERROR_RECOVERY_OR_PARTIAL_SUCCESS}}

## Authorization and Security

- Actors and permissions: {{AUTHORIZATION_RULES}}
- Trust boundaries: {{TRUST_BOUNDARIES}}
- Sensitive data: {{HANDLING_REQUIREMENTS}}
- Abuse/security expectations: {{SECURITY_REQUIREMENTS}}

## Data Lifecycle

{{CREATE_READ_UPDATE_RETENTION_DELETION_CONSISTENCY_AND_RECOVERY}}

## Compatibility

{{BACKWARD_FORWARD_PUBLIC_INTERFACE_AND_COEXISTENCE_REQUIREMENTS}}

## Non-Functional Requirements

- {{CONSTRAINT_AND_MEASURABLE_BOUND}}

## Observability

{{LOGGING_METRICS_TRACING_AUDIT_ALERTS_OR_NOT_APPLICABLE}}

## Migration and Rollback

{{MIGRATION_ROLLOUT_ROLLBACK_AND_SAFETY_OR_NOT_APPLICABLE}}

## Acceptance Criteria

| ID | Criterion | Verification | Evidence required |
|---|---|---|---|
| AC-001 | {{TESTABLE_CRITERION}} | `{{COMMAND_OR_CHECK}}` | {{EXPECTED_EVIDENCE}} |

## Non-Goals

- {{EXPLICIT_NON_GOAL}}

## Assumptions

- {{ASSUMPTION_AND_CEILING}}

## Open Questions

See `.agent/open-questions.md`. No unresolved blocking question may materially alter public behavior, architecture, storage, security, compatibility, ownership, or graph structure.

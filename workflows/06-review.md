# Phase 6: Independent Review and Approval

## Purpose

Evaluate the integrated result in fresh context, route evidence-based findings, and prepare a truthful approval summary.

## Inputs

- User objective and `.agent/spec.md`.
- `.agent/decisions.md` and graph summary.
- Final integration diff and commit/patch.
- Node and integration handoffs.
- `.agent/reports/verification.md`.
- Known assumptions, blockers, and risks.

## Required Context

Provide only the accepted objective/specification, relevant decisions, graph summary, final diff, verification evidence, handoffs, assumptions, and risks. Do not provide implementation-agent reasoning or conclusions as authoritative.

## Procedure

1. Dispatch a reviewer in fresh context with the canonical review prompt.
2. Evaluate:
   - requirement and acceptance coverage;
   - functional correctness and error handling;
   - authorization and security;
   - backward/forward compatibility;
   - concurrency behavior;
   - data consistency and lifecycle;
   - migration and rollback safety;
   - test quality and verification gaps;
   - unnecessary complexity;
   - path, contract, and scope violations.
3. Require evidence, severity, blocking status, owner node, description, recommended action, and at least one location (`file` or `component`) for every finding. Either location may be `null`, but not both. Allowed severities are `critical`, `high`, `medium`, and `low`; any finding with `blocking: true` requires `changes_required`.
4. Write `.agent/reports/review.json` using only `approved`, `approved_with_notes`, or `changes_required`.
5. Route every blocking or `changes_required` finding to its owner. Repair in Phase 4, reintegrate in Phase 5, then run a new fresh review.
6. Do not approve while any blocking finding remains.
7. Write `.agent/reports/final-summary.md` covering objective, completed nodes/commits, behavior and migration changes, verification, resolved findings, remaining notes, assumptions, risks, rollback/delivery guidance, and each action requiring approval.
8. Mark the review graph node completed only after the review artifact validates and has no blocking finding.
9. Set workflow state to `awaiting_approval` when delivery actions remain unauthorized.

## Outputs

- `.agent/reports/review.json`
- `.agent/reports/final-summary.md`
- Updated `.agent/state.json`

## Completion Conditions

- Review context was fresh and implementation conclusions were treated as untrusted.
- Review is `approved` or `approved_with_notes` with no blocking finding.
- Final summary is evidence-backed and names unresolved risks and approval-gated actions.

## Blocking Conditions

- Review returns `changes_required`.
- Required final diff, specification, handoff, or verification evidence is missing.
- A finding has no clear owner and ownership cannot be assigned.
- Security, migration, compatibility, or data risk requires a human decision.

## Transition

Stop at `awaiting_approval` unless the user explicitly requests delivery preparation or authorizes a Phase 7 action. Performing delivery requires action-specific authorization.

## Required References

- [Context Management](../references/context-management.md)
- [Failure Handling](../references/failure-handling.md)
- [Approval Boundaries](../references/approval-boundaries.md)
- [Workflow State](../references/workflow-state.md)

## Required Templates

- [Review](../templates/review.json)
- [Final Report](../templates/final-report.md)
- [Review Agent Prompt](../prompts/review-agent.md)
- [Repair Agent Prompt](../prompts/repair-agent.md) when findings require repair.

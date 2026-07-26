# Phase 5: Integration and Verification

## Purpose

Combine validated implementation outputs, verify cross-component behavior, and produce the final integration evidence without redesigning owned components.

## Inputs

- Every validated dependency artifact/handoff required by the integration contract.
- Validated commits or patches.
- `.agent/spec.md`, `.agent/decisions.md`, `.agent/graph.json`, and `.agent/state.json`.
- Repository verification inventory.

## Required Context

- Integration node contract.
- Final interfaces and dependency handoffs.
- Relevant specifications, migration rules, known assumptions, and risks.
- Repository-level format, lint, build, test, and integration commands.

## Procedure

1. Confirm every dependency in the integration contract, regardless of node type, has its contracted completed and valid artifact/handoff.
2. Create/select the isolated integration workspace and record its base.
3. Combine validated commits or patches in dependency order.
4. Resolve mechanical conflicts such as imports, generated ordering, or non-semantic formatting.
5. Route semantic defects, incompatible contracts, and behavior changes to the original owner; do not redesign them inside integration without an explicit ownership transfer.
6. Define `files_changed` in the integration handoff as the complete combined diff. Validate inherited paths against their upstream handoffs and scopes. Record only integration-authored edits in `integration_changes`, and apply the integration node's allowed/forbidden paths to those edits.
7. Run required formatting, linting, static analysis, builds, and repository-level tests.
8. Verify cross-component contracts and integration scenarios.
9. Evaluate every accepted criterion against combined behavior.
10. Run migration validation, forward/backward or rollback checks, and compatibility/coexistence checks when relevant.
11. Attribute each failure to its owning implementation node or to integration mechanics.
12. Route owner defects through repair, re-combine the repaired result, and rerun affected plus full verification.
13. Write `.agent/reports/verification.md` with commands, environment/prerequisites, exit codes, results, skipped checks, and evidence.
14. Write and validate `.agent/handoffs/integration.json`, including integration-specific changes and final commit/patch. Each `integration_changes` item contains `paths`, `rationale`, and diff/commit `evidence`.
15. Mark the integration graph node completed in state only after the handoff and verification report validate.

The integration owner may combine validated owner branches/patches but must not rebase or rewrite those branches. Its own edits remain limited to the integration contract's allowed paths.

## Outputs

- `.agent/handoffs/integration.json`
- `.agent/reports/verification.md`
- Final integrated commit or patch set.
- Updated `.agent/state.json`.

## Completion Conditions

- Required commits/patches are combined.
- No blocking format, lint, build, test, contract, acceptance, integration, compatibility, or migration failure remains.
- Integration-only changes are recorded.
- The integration handoff and verification report validate.

## Blocking Conditions

- Required implementation output is absent or invalid.
- Contracts are semantically incompatible.
- A migration or compatibility check fails.
- A pre-existing repository failure prevents trustworthy integration evidence.
- Repair limit is exhausted.
- Integration would require an unapproved architecture or public-interface change.

## Transition

Proceed only to Phase 6: Independent Review and Approval. Semantic repairs return to Phase 4 and then repeat this phase.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Context Management](../references/context-management.md)
- [Failure Handling](../references/failure-handling.md)
- [Parallel Execution](../references/parallel-execution.md)

## Required Templates

- [Handoff](../templates/handoff.json)
- [Integration Agent Prompt](../prompts/integration-agent.md)

# Phase 4: Node Execution

## Purpose

Execute each ready pre-integration node in isolation, validate its evidence, persist its handoff, and route repairs without leaking ownership.

## Inputs

- `.agent/graph.json`
- `.agent/state.json`
- `.agent/tasks/<node-id>.md`
- Required dependency handoffs and decisions.

## Required Context

- Complete node contract.
- Repository rules and specialist instructions.
- Relevant specification excerpts and source paths.
- Validated dependency handoffs, decisions, branch state, acceptance criteria, verification commands, and handoff schema.

## Procedure

1. Recompute readiness from persisted graph/state. Start only a node whose dependencies and required artifacts validate and whose workspace/path/external-resource scope does not collide with a running node.
2. Create or select the node's isolated worktree/branch. A write-enabled agent must not share another writer's mutable workspace.
3. Build the minimum sufficient context package and confirm allowed/forbidden paths before dispatch.
4. Invoke the specialist using the canonical prompt and node contract.
5. Require inspection of existing conventions before modification.
6. Restrict edits to allowed paths and prohibit silent public-interface changes or weakened tests.
7. Require focused implementation checks and every contract verification command.
8. For write-enabled nodes or contracts requiring repository changes, require a commit in a Git worktree or a durable patch reference. Read-only nodes may report both as `null` when their declared artifact and verification evidence exist.
9. Require `.agent/handoffs/<node-id>.json` with:
   - status and summary;
   - consumed inputs;
   - actual files changed;
   - commit or patch;
   - interfaces created/changed;
   - commands, exit codes, and results;
   - every acceptance criterion, status, and evidence;
   - assumptions, risks, blockers, and follow-up.
10. Validate the handoff before accepting it:
    - parse every required field and allowed status;
    - for write-enabled nodes, inspect the actual commit/patch diff, including deletes and renames;
    - compare actual paths with allowed and forbidden scopes;
    - confirm artifact and reported files/references exist;
    - for write-enabled nodes, confirm the commit resolves or patch is durable; for read-only nodes, confirm the declared artifact/evidence instead;
    - inspect actual command results;
    - require passed evidence for every criterion.
11. Reject `completed` when verification was not executed, evidence is missing, scope was exceeded, the artifact is absent, or a required commit/patch cannot be resolved.
12. Update state safely only after validation; then unlock dependents whose complete dependency sets validate.
13. On verification failure, return relevant evidence to the same owner, increment attempts, and use the repair prompt.
14. Stop at `max_retries`; do not assign an unrelated agent unless ownership is explicitly transferred.
15. Persist blockers/failures and stop affected downstream scheduling.

## Outputs

- `.agent/handoffs/<node-id>.json` for every attempted node.
- Updated `.agent/state.json`.
- Commits or patches owned by write-enabled nodes when contracted.

## Completion Conditions

- Every dependency of integration, regardless of node type, has its contracted valid completion artifact.
- Each completed write-enabled node has verification evidence, acceptance results, actual files changed, commit/patch, assumptions, risks, blockers, and interface changes.
- Each completed read-only node has its declared artifact, verification evidence, assumptions, risks, and blockers; commit and patch may both be `null`.
- No downstream node was unlocked from prose alone.

## Blocking Conditions

- Required artifact is missing or incompatible.
- Completion requires forbidden paths.
- Requirements contradict repository behavior.
- Verification cannot run because of an external dependency.
- Completion requires an unapproved material decision.
- Retry limit is exhausted.

Classify and handle these through the failure reference; do not hide them in a completed handoff.

## Transition

Proceed to Phase 5 only after every integration dependency validates. A repaired node re-enters this phase at `needs_repair -> running`.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Context Management](../references/context-management.md)
- [Parallel Execution](../references/parallel-execution.md)
- [Failure Handling](../references/failure-handling.md)

## Required Templates

- [Node Contract](../templates/node-contract.md)
- [Handoff](../templates/handoff.json)
- [Specialist Agent Prompt](../prompts/specialist-agent.md)
- [Repair Agent Prompt](../prompts/repair-agent.md)

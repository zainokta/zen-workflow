# Phase 3: Graph Planning

## Purpose

Decide whether a graph is justified and, when it is, produce an executable dependency graph of concrete, isolated deliverables.

## Inputs

- `.agent/spec.md`
- `.agent/decisions.md`
- `.agent/context.md`
- `.agent/repository-map.json`
- `.agent/open-questions.md`

## Required Context

- Accepted criteria and decisions.
- Affected repository map, relevant paths, existing patterns, and verification inventory.
- Open questions required before execution.

## Procedure

1. Reapply the root graph-use gate. If overhead is unjustified, record the reason in state and use one focused implementation loop.
2. Decompose work by concrete deliverable, interface, or artifact—not by seniority or generic roles.
3. Use only supported node types: `research`, `design`, `implementation`, `verification`, `integration`, `review`, `documentation`.
4. Create research/design nodes only when a downstream node consumes their artifact.
5. Assign one repository ID, owner, objective, output artifact, and bounded responsibility to every node. Persist a global repository map with path, base, and base commit for every stable repository ID.
6. Add an edge only when the downstream node consumes an upstream interface, decision, artifact, commit/patch, or verification result; record that reason.
7. Define minimum context inputs, allowed paths, forbidden paths, acceptance criteria, verification commands, maximum retries, and concurrency group.
8. Compare implementation path scopes, including generated, renamed, and deleted files. Combine, sequence, or extract shared changes when scopes overlap.
9. Identify shared mutable external resources and serialize nodes that could conflict.
10. Require stable output contracts before placing consumers in the same concurrency wave.
11. Define one isolated worktree/branch strategy per write-enabled node, or an equivalent isolated workspace when Git worktrees are unavailable.
12. Include an explicit integration node after implementations and an independent review node after integration.
13. Reject duplicate IDs, unknown dependencies, cycles, unsupported types, missing artifacts, missing criteria/verification, or implementation nodes without bounded paths.
14. Create a state entry for every graph node, including integration and review, with repository ID, owner, attempts, artifact, commit/patch, workspace/base/HEAD provenance, external resources, timestamps, and blockers. Derive waiting/ready status from dependencies.
15. Materialize every node contract under `.agent/tasks/`.

Avoid nodes whose only output is “thought,” whose artifact is unused, or whose context is nearly the whole repository.

## Outputs

- `.agent/graph.json`
- `.agent/state.json`
- `.agent/tasks/<node-id>.md`

## Completion Conditions

- Graph justification or fallback decision is persisted.
- Every node and edge is valid, useful, owned, bounded, verifiable, and artifact-producing.
- Parallel groups have no unresolved dependency, path, workspace, contract, or external-resource conflict.
- Integration and independent review are explicit.

## Blocking Conditions

- A graph-shaping specification question is unresolved.
- Safe ownership cannot be expressed without overlapping writers.
- Required contracts are unstable.
- Verification commands or acceptance evidence cannot be defined.
- The graph contains a cycle or requires unavailable isolation.

## Transition

When a graph is valid, proceed to Phase 4 for ready nodes only. When graph execution is rejected, follow the persisted single-loop fallback and retain the same verification and approval boundaries.

## Required References

- [Workflow State](../references/workflow-state.md)
- [Context Management](../references/context-management.md)
- [Parallel Execution](../references/parallel-execution.md)
- [Failure Handling](../references/failure-handling.md)

## Required Templates

- [Graph](../templates/graph.json)
- [State](../templates/state.json)
- [Node Contract](../templates/node-contract.md)

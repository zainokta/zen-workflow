---
name: zen-workflow
description: Use when a software-engineering objective spans multiple components, repositories, or independent workstreams and benefits from dependency-aware delegation, isolated worktrees, parallel agents, persisted handoffs, integration verification, and fresh-context review. Do not use for a small change that one focused agent can safely implement and verify.
---

# Zen Workflow

Execute complex engineering work through a dependency-aware graph of isolated agent workflows. The orchestrator owns intake, discovery, specification, planning, scheduling, validation, integration, review, and approval routing; specialist agents own bounded deliverables.

## When to use

Use graph execution when at least two apply:

- multiple components or repositories must change;
- two or more deliverables have independent ownership;
- safe parallelism materially reduces elapsed time;
- separate domain expertise or fresh review is valuable;
- one context would contain substantial irrelevant material;
- implementation nodes can own non-overlapping paths;
- implementation and review should be isolated.

## When not to use

Use one focused implementation loop when:

- the change is confined to one small area;
- one short command verifies the complete change;
- most work would touch the same files;
- task boundaries or artifacts cannot be stated clearly;
- every subagent would need nearly identical context;
- graph overhead is greater than the work.

If discovery shows the graph is unjustified, record the decision and fall back instead of inventing nodes.

## Global invariants

- Agents do not share full conversation histories.
- Agents share contracts, accepted decisions, relevant context, dependency artifacts, persisted state, commits or patches, and verification evidence.
- Conversation history is never workflow state.
- Load only the current phase workflow, relevant global references, and templates required for the current operation.
- Implementation nodes receive the minimum sufficient context for their responsibility.
- Every node owns a concrete deliverable, one output artifact, and bounded paths.
- Completion claims require inspected command and artifact evidence.
- A downstream node remains locked until all dependencies have valid completed handoffs.
- Concurrent write-enabled agents never share a mutable workspace.
- Parallel nodes require satisfied dependencies, non-overlapping paths, isolated workspaces, stable contracts, and no shared mutable external resource.
- Destructive or irreversible actions require explicit human approval.

## Source of truth

Use this precedence:

1. repository contents and repository instructions;
2. accepted specifications;
3. accepted decisions;
4. graph and workflow state;
5. node contracts;
6. handoff artifacts;
7. Git commits or patches;
8. verification evidence.

If sources conflict, stop the affected work, record the conflict, and resolve it in the earliest phase whose accepted output must change. Do not treat agent prose as authoritative.

Follow [Workflow State](references/workflow-state.md) whenever creating, reading, updating, or recovering `.agent/` artifacts.

## Lifecycle

```text
Phase 0: Intake and Interview
Phase 1: Technical Discovery
Phase 2: Specification and Decisions
Phase 3: Graph Planning
Phase 4: Node Execution
Phase 5: Integration and Verification
Phase 6: Independent Review and Approval
Phase 7: Delivery preparation when requested; delivery actions only when explicitly approved
```

Apply the when/when-not gate before Phase 0. If the task clearly belongs in one focused loop, exit this skill without creating `.agent/` graph artifacts. When graph candidacy remains, do not skip intake, discovery, or specification merely because an implementation seems obvious; scale them to the work and satisfy their completion conditions before graph planning.

## Phase routing

### Phase 0: Intake and Interview

- Load: [workflows/00-intake.md](workflows/00-intake.md)
- Primary input: user objective and supplied requirements.
- Required outputs: `.agent/intake.md`, `.agent/open-questions.md`.
- Complete when: intended outcome, scope, actors, observable behavior, definition of done, assumptions, and blocking questions are classified.
- Next permitted phase: Phase 1.

### Phase 1: Technical Discovery

- Load: [workflows/01-discovery.md](workflows/01-discovery.md)
- Primary input: accepted intake and repository.
- Required outputs: `.agent/context.md`, `.agent/repository-map.json`.
- Complete when: affected components, conventions, integration points, risks, and runnable verification commands are evidenced without implementing application code.
- Next permitted phase: Phase 2.

### Phase 2: Specification and Decisions

- Load: [workflows/02-specification.md](workflows/02-specification.md)
- Primary input: intake and discovery artifacts.
- Required outputs: `.agent/spec.md`, `.agent/decisions.md`, updated `.agent/open-questions.md`.
- Complete when: behavior, errors, authorization, security, data, compatibility, non-goals, and testable criteria are accepted, with no graph-shaping blocking question unresolved.
- Next permitted phase: Phase 3.

### Phase 3: Graph Planning

- Load: [workflows/03-graph-planning.md](workflows/03-graph-planning.md)
- Primary input: accepted specification, decisions, and repository context.
- Required outputs: `.agent/graph.json`, `.agent/state.json`, `.agent/tasks/<node-id>.md`.
- Complete when: every useful node has an owner, artifact, dependency reason, bounded paths, criteria, commands, retries, and safe execution strategy; or graph execution is rejected in favor of one focused loop.
- Next permitted phase: Phase 4 when a graph is justified; otherwise the recorded single-loop path.

### Phase 4: Node Execution

- Load: [workflows/04-node-execution.md](workflows/04-node-execution.md)
- Primary input: one ready pre-integration node contract and its validated dependency artifacts.
- Required output per node: `.agent/handoffs/<node-id>.json`.
- Complete when: every dependency of the integration node has its contracted valid completion artifact, or execution has stopped with a persisted blocker/failure report.
- Next permitted phase: Phase 5.

### Phase 5: Integration and Verification

- Load: [workflows/05-integration.md](workflows/05-integration.md)
- Primary input: validated implementation commits or patches and validated handoffs.
- Required outputs: `.agent/handoffs/integration.json`, `.agent/reports/verification.md`.
- Complete when: combined changes pass required repository-level, contract, acceptance, integration, and migration checks with no blocking failure.
- Next permitted phase: Phase 6.

### Phase 6: Independent Review and Approval

- Load: [workflows/06-review.md](workflows/06-review.md)
- Primary input: accepted specification, final integration diff, handoffs, and verification evidence.
- Required outputs: `.agent/reports/review.json`, `.agent/reports/final-summary.md`.
- Complete when: fresh review has no blocking finding and the summary identifies every remaining risk and approval-gated action.
- Next permitted phase: stop at `awaiting_approval`, or Phase 7 after an explicit preparation request or action-specific authorization.

### Phase 7: Delivery

- Load only after an explicit request to prepare delivery or explicit action-specific authorization to perform it: [workflows/07-delivery.md](workflows/07-delivery.md)
- Primary input: the requested preparation or authorized action, reviewed integration result, final summary, and delivery constraints.
- Required outputs: action-specific preparation, execution, rollback, and post-action evidence.
- Complete when: only requested preparation or authorized actions have finished and their evidence is recorded; preparation alone may complete with delivery still pending.
- Next permitted phase: none.

## Global lifecycle gates

| Gate | Required condition |
|---|---|
| Plan implementation graph | Intake, discovery, and specification are sufficiently complete |
| Start node | Node is `ready`; every dependency handoff validates |
| Run parallel writers | Paths and external resources do not conflict; each writer has an isolated workspace |
| Unlock downstream | Required handoffs, commits or patches, commands, criteria, and evidence validate |
| Complete integration | No blocking format, lint, build, test, contract, acceptance, or migration failure |
| Approve result | Fresh review has no unresolved blocking finding |
| Prepare delivery | Preparation artifact is explicitly requested and labeled `not executed` |
| Deliver | Exact merge, deploy, migration, infrastructure, data, credential, or permission action is explicitly authorized |

## Operating rules

1. Apply the root gate; exit to one focused loop when graph execution clearly does not apply.
2. Otherwise enter at Phase 0 unless valid persisted artifacts allow safe recovery at a later phase.
3. Load the routed workflow and only its named references/templates.
4. Validate required inputs before executing that phase.
5. Persist outputs before transitioning.
6. Reopen the earliest affected phase when new evidence invalidates an accepted artifact.
7. Use [Failure Handling](references/failure-handling.md) for retries, blockers, incompatible contracts, or invalid plans.
8. Use [Approval Boundaries](references/approval-boundaries.md) before any consequential external action.

## Overall completion

The zen-workflow is complete when:

- every graph node has its canonical valid completion artifact: node handoff, integration handoff, or review report;
- integration verification passes;
- independent review has no unresolved blocking finding;
- state, decisions, contracts, handoffs, reports, and evidence are persisted;
- behavior changes, assumptions, risks, rollback guidance, and approval requirements are reported.

Completion may intentionally end at `awaiting_approval`. Delivery is not required unless the user explicitly authorizes it. Never mark merge or deployment complete when only preparation was authorized.

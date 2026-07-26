# Workflow State

Use persisted artifacts to coordinate phases, nodes, recovery, and evidence. Conversation history is never state.

## Artifact layout

```text
.agent/
├── intake.md
├── open-questions.md
├── context.md
├── repository-map.json
├── spec.md
├── decisions.md
├── graph.json
├── state.json
├── tasks/
│   └── <node-id>.md
├── handoffs/
│   ├── <node-id>.json
│   └── integration.json
└── reports/
    ├── verification.md
    ├── review.json
    └── final-summary.md
```

Repositories may define another standard; record and use one canonical location consistently.

## Source-of-truth order

1. Repository contents and instructions.
2. Accepted specifications.
3. Accepted decisions.
4. Graph and workflow state.
5. Node contracts.
6. Handoff artifacts.
7. Git commits or patches.
8. Verification evidence.

A later item cannot silently override an earlier one. Reopen the owning phase when evidence invalidates an accepted artifact.

## Workflow state

Canonical workflow statuses:

- `in_progress`
- `blocked`
- `failed`
- `awaiting_approval`
- `completed`

Canonical phases:

- `0-intake`
- `1-discovery`
- `2-specification`
- `3-graph-planning`
- `4-node-execution`
- `5-integration`
- `6-review`
- `7-delivery`

Normal phase transitions:

```text
0-intake -> 1-discovery -> 2-specification -> 3-graph-planning
-> 4-node-execution -> 5-integration -> 6-review
-> awaiting_approval
-> 7-delivery after an explicit preparation request or action-specific authorization
```

New evidence may move back to the earliest phase whose accepted output must change. Record why, invalidate dependent artifacts, and do not keep downstream state marked complete.

## Node state

Canonical node transitions:

```text
waiting -> ready
ready -> running
running -> completed
running -> blocked
running -> failed
failed -> ready
blocked -> ready
completed -> needs_repair
needs_repair -> running
```

Do not invent aliases in graph/state/templates. Overall workflow status and node status are separate fields.

Use `blocked -> ready` only after the blocker clears and dependencies, workspace, contracts, and required artifacts are revalidated. Clearing a blocker does not consume a repair retry; a new dispatched execution increments total `attempts`.

Handoff status is a separate artifact field with allowed values `completed`, `blocked`, `failed`, and `needs_review`. `needs_review` means the node produced its contracted work/evidence but requests an orchestrator or decision-owner ruling before completion; it never unlocks dependents. Keep the node blocked until the ruling causes the owner to update the handoff to `completed`, `failed`, or `blocked`. Review status is separate again with allowed values `approved`, `approved_with_notes`, and `changes_required`: use `changes_required` when any finding is blocking, `approved_with_notes` only when every finding is non-blocking, and `approved` when no finding requires a note. Do not use a handoff or review status as a node-state alias.

## Dependency unlocking

A node is `ready` only when:

- every `depends_on` node is `completed`;
- every required dependency handoff parses and validates;
- required commits/patches and artifacts resolve;
- no accepted decision or open blocking question prevents execution;
- its concurrency/workspace constraints can be satisfied.

Recompute readiness from graph plus artifacts. Never unlock from a chat message or a handoff's status field alone.

## Safe updates

Keep `graph.json` stable during execution unless the plan is explicitly revised. For `state.json`:

1. read the latest persisted state;
2. validate the proposed transition;
3. write a complete valid replacement through the safest atomic mechanism available;
4. reread and parse it;
5. record timestamps, attempts, artifact, commit/patch, and blocker changes together.

Use a temporary file plus atomic rename on filesystems where that is reliable. Avoid multiple orchestrators writing state concurrently; one orchestrator owns scheduling/state.

## Recovery after interruption

1. Read repository instructions and all existing `.agent/` artifacts relevant to the recorded phase.
2. Verify the repository/worktree HEADs and commit/patch references.
3. Validate JSON and cross-check artifacts against actual diffs and command evidence.
4. Recompute phase and node readiness; do not trust a stale `running` marker.
5. Classify interrupted `running` nodes:
   - resume only when workspace, owner, and attempt evidence are recoverable;
   - otherwise mark `failed` or `blocked` with reason and recoverable artifacts.
6. Continue from the earliest incomplete valid phase.

## Stale-state detection

Treat state as stale when:

- repository HEAD/base differs from recorded values;
- artifact mtime/content or commit/patch does not match state;
- a dependency changed after a downstream handoff;
- a recorded worktree no longer exists;
- a `running` node has no active owner or recoverable output;
- accepted specification/decision changed without dependent invalidation;
- verification evidence belongs to a different commit.

On stale state, stop scheduling, record the mismatch, invalidate affected downstream results, and recover or replan.

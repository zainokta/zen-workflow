# Zen Workflow

A small escalation workflow for engineering tasks that genuinely benefit from a dependency graph.

Default to one focused implementation loop. Use graph mode only for multi-repository work, at least three independent non-overlapping deliverables, materially useful specialist parallelism, or high-risk work needing isolated review.

## Usage

```text
Use $zen-workflow for this engineering objective. Choose focused or graph mode using its gate, then execute and verify the work.
```

## Modes

### Focused

Inspect the real flow, make the smallest root-cause change, run meaningful checks, inspect the diff, and report the result. No workflow artifacts.

### Graph

Persist only the state needed for safe coordination:

```text
.agent/
├── spec.md
├── graph.json
├── handoffs/<node-id>.json
└── verification.md
```

`graph.json` contains both node contracts and state. Writers use isolated worktrees, parallel nodes cannot overlap paths or external resources, and dependents unlock only from validated handoffs. Integration and fresh review are added when the task justifies them.

See [SKILL.md](SKILL.md) for the complete operating contract.

---
name: zen-workflow
description: "Use for complex engineering work that genuinely benefits from a dependency graph: multiple repositories, at least three independent non-overlapping deliverables, or high-risk work needing isolated implementation and review. Default to one focused loop."
---

# Zen Workflow

Use the smallest workflow that safely finishes the objective. A graph is an escalation path, not the default.

## Understand first

Inspect the objective, repository instructions, affected code, callers, tests, and integration points before choosing a workflow. Find repository facts yourself. Ask only when a wrong assumption would materially change behavior, security, data, compatibility, ownership, or delivery.

## Choose

Use a **focused loop** unless at least one trigger is clear:

- multiple repositories must change;
- at least three implementation deliverables can own non-overlapping paths;
- independent specialist work materially reduces elapsed time;
- high-risk security, data, infrastructure, or compatibility work benefits from isolated implementation and review.

Do not use a graph when work overlaps the same files, workers need nearly identical context, one owner can verify everything directly, or coordination costs more than implementation.

## Focused loop

1. Trace the real flow end to end.
2. Make the smallest root-cause change.
3. Run the narrowest meaningful check and required repository-level checks.
4. Inspect the final diff and report behavior, verification, risks, and approval-gated actions.

Create no workflow artifacts. Stop here unless the graph gate is met.

## Graph mode

One orchestrator owns scheduling and shared state. Workers own bounded deliverables.

Create only:

```text
.agent/
├── spec.md
├── graph.json
├── handoffs/<node-id>.json
└── verification.md
```

Never create empty artifacts. Keep decisions and blocking questions in `spec.md`; keep node contracts and state in `graph.json`. Add files only when the repository requires them or their content cannot fit clearly here.

### Specify

`spec.md` contains the objective, observable definition of done, scope, non-goals, behavior and constraints, acceptance criteria, required repository-level checks, and unresolved blockers. Do not plan nodes while a graph-shaping blocker remains.

### Plan

`graph.json` is one valid document with repository base commits and nodes. Each node contains:

- unique ID, type, objective, repository ID, dependencies, and status;
- allowed/forbidden paths and shared external resources;
- acceptance criteria, verification commands, and handoff path;
- workspace path, branch, base, attempts, maximum attempts, and blocker.

Supported types: `research`, `implementation`, `integration`, `review`. Create research only when a downstream node consumes its artifact. Every node needs a concrete output and verification; no “thinking” nodes.

Add an edge only when its consumer needs an upstream artifact, interface, commit, or result. Reject duplicate IDs, unknown dependencies, cycles, missing checks, and overlapping concurrent writers. Use an integration node after multiple writers. Use fresh review for high-risk work or when independent review is requested or materially useful.

### Schedule safely

A node becomes ready only after every dependency has a validated completed handoff. Parallel nodes must have non-overlapping paths, isolated writer workspaces, stable interfaces, and no shared mutable database, infrastructure state, environment, port, service, credential, or generated output.

If writers may collide, combine them, sequence them with a real dependency, or extract the shared change upstream. Never ask concurrent agents to coordinate edits to the same file.

Prefer one worktree per writer:

```bash
git worktree add ../worktrees/<node-id> -b agent/<node-id> <base>
```

### Keep context small

Give a worker only repository instructions, its node contract, relevant spec excerpts and source paths, validated dependency handoffs, workspace/base/HEAD, and the handoff shape. Exclude the parent conversation, unrelated files, duplicate specs, and raw large logs. Send focused errors plus a durable log path.

Do not spawn an agent when the orchestrator can perform the step with less context and equal independence. If a worker needs most of the repository or history, redraw or merge the node.

### Validate handoffs

Each attempted node writes `.agent/handoffs/<node-id>.json` with:

- node ID, status (`completed`, `blocked`, or `failed`), and summary;
- files changed and resolvable commit or durable patch for write nodes;
- commands with exit codes and results;
- every criterion with status and evidence;
- interface changes, assumptions, risks, and blockers.

Before accepting completion, inspect the actual diff, path scope, artifact, command output, and criterion evidence. Prose alone never unlocks dependents.

Only the orchestrator updates `graph.json`: read the latest file, validate the transition, replace atomically when possible, then parse it again.

### Repair, integrate, review

Return failures to the same owner with focused evidence. Increment attempts only when dispatching. Default to two attempts; then persist the blocker and stop affected dependents.

When evidence invalidates a spec, dependency, handoff, or result, reopen the earliest affected work and invalidate downstream completion.

The integration owner combines validated commits or patches without rewriting owner branches, runs acceptance and repository-level checks, and records commands, exit codes, evidence, integrated HEAD, and risks in `verification.md`.

A fresh reviewer receives the objective, spec, final diff, handoffs, and verification report—not implementation reasoning as authority. Blocking findings return to the owning node, then integration and review rerun.

Complete when outputs exist, checks pass, the final diff is inspected, blocking findings are resolved, and remaining risks are reported.

## Approval boundary

Inspection, local edits, tests, commits, and requested delivery preparation are allowed. Merge, deploy, production mutation, migration execution, infrastructure, credentials, permissions, destructive actions, and irreversible external actions require explicit action-specific authorization.

Preparation is not execution. Never report an approval-gated action as completed when only its plan or artifact exists.

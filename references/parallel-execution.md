# Parallel Execution

Parallelism is allowed only when it preserves dependency correctness, file ownership, workspace isolation, contract stability, and external-resource safety.

## Readiness

Nodes may run concurrently when:

- no unresolved dependency edge exists between them;
- every upstream artifact needed by either node validates;
- output contracts consumed later are already stable;
- no open decision can change their scopes or interfaces.

Feature membership alone neither requires sequencing nor permits parallelism.

## Paths

Compare allowed paths before dispatch, including likely generated, moved, renamed, and deleted files.

If two write nodes may touch the same path:

1. combine them under one owner;
2. sequence them with a real dependency; or
3. extract the shared change into an upstream node.

Do not rely on agents to “coordinate” concurrent edits to the same file.

## Workspace isolation

Prefer one Git worktree and branch per write-enabled node:

```bash
git worktree add ../worktrees/<node-id> -b agent/<node-id> <base>
```

Each node:

- works only in its assigned workspace;
- modifies only allowed paths;
- commits its own work or produces the contracted patch;
- reports the commit/patch;
- does not rebase, rewrite, or merge another node's branch, except that the Phase 5 integration owner may combine validated inputs without rebasing or rewriting owner branches;
- uses isolated build caches, ports, databases, container names, and temporary resources when needed.

Read-only research/review nodes need no worktree unless their commands mutate state.

## External resources

Sequence nodes that mutate the same:

- database/schema instance;
- infrastructure state or state lock;
- deployment environment;
- queue/topic/bucket or other shared service;
- credentials, permissions, or external configuration;
- code-generation output.

Use disposable isolated resources for tests when possible. Never treat an external resource as safe merely because file paths do not overlap.

## Concurrency groups

Assign a concurrency group to express an intended safe wave. A group is scheduling metadata, not proof of safety. Before dispatch, revalidate dependencies, paths, workspace, contracts, and external resources against current state.

## Sequential conditions

Run sequentially when:

- one node defines an interface consumed by another;
- paths overlap;
- one produces generated code used by another;
- nodes mutate the same schema/infrastructure/external resource;
- a shared architectural or security decision is unresolved;
- integration evidence from one is required to scope another.

Integration is always an explicit downstream phase after every dependency in its contract.

# Failure Handling

Failures stay with the owner, carry evidence, consume bounded retries, and stop unsafe downstream work.

## Classify status

Use `blocked` when progress requires an unavailable dependency, external change, approval, compatible contract, authorized decision, or forbidden-path ownership change.

Use `failed` when the owner attempted the contracted work or verification and did not complete it within the current attempt.

Do not mark `completed` with skipped required verification, missing evidence, unresolved blockers, out-of-scope changes, or an unresolved commit/patch.

## Verification failure

1. Identify the node that owns the failing behavior or artifact.
2. Capture the failed command, exit code, relevant output, affected criteria, and affected files/components.
3. Send only that evidence and required context to the same owner using the repair prompt.
4. Increment attempts.
5. Repair only inside allowed paths.
6. Rerun affected checks and every command required by the node contract.
7. Update the existing handoff and validate it again.

Do not transfer repair work unless ownership is explicitly reassigned and graph/state/contracts are updated.

## Retry limits

`max_retries` is the number of repair dispatches allowed after the initial execution. State `attempts` counts every dispatch, including the initial execution, so it may not exceed `1 + max_retries`. Repeating the same action without new evidence still consumes an attempt.

Escalate earlier when another attempt cannot change the blocking condition or would require unauthorized scope.

## Incompatible dependencies

When a consumer finds an incompatible upstream contract:

1. stop the consumer;
2. compare accepted specification/decision with both contracts;
3. assign the semantic defect to the owner whose output violates the accepted contract;
4. if the accepted contract itself is ambiguous or wrong, reopen specification/decision and invalidate affected downstream artifacts;
5. repair, reintegrate, and reverify.

Integration may fix mechanical conflicts. It routes semantic defects to original owners.

## Pre-existing repository failures

Record the exact baseline command/evidence and whether it prevents verification of the requested change. Do not claim the new work passes through an obstructing baseline failure.

- If focused evidence remains trustworthy, continue and report the unrelated failure as risk.
- If it blocks trustworthy verification, stop and ask for direction or create an explicitly approved prerequisite node.

## Invalid graph plan

Stop scheduling and return to graph planning when architecture, ownership, paths, contracts, dependencies, or external resources make the current graph unsafe or incorrect. Invalidate downstream ready/running assumptions before revising.

## Stopping conditions

Stop graph execution for:

- contradictory blocking requirement;
- unavailable required credentials, infrastructure, or repository;
- destructive/irreversible action awaiting approval;
- exhausted retry limit;
- unreconcilable contracts;
- blocking pre-existing failure;
- architecture that invalidates the graph;
- unowned blocking review finding.

## Failure report

Report:

- failed/blocked phase and node;
- attempted actions and attempt count;
- command/artifact evidence;
- affected criteria;
- current repository/worktree and state;
- recoverable commits, patches, and artifacts;
- blockers and owner;
- recommended next action and approval required.

# Approval Boundaries

Approval is action-specific. Permission to inspect, plan, implement locally, test, or prepare delivery artifacts does not imply permission to perform an external or irreversible action.

## Explicit approval required

- Merge into a protected or primary branch.
- Deployment to any shared or production environment.
- Destructive or irreversible migration.
- Public API breaking change.
- Security-sensitive behavior change.
- Irreversible infrastructure operation.
- Production data mutation, deletion, backfill, or repair.
- Credential, secret, permission, role, policy, or access-control change.

Repository rules or organizational policy may require approval for additional actions; preserve the stricter boundary.

## Preparation versus performance

Without delivery authorization, agents may prepare:

- merge/PR summary;
- release notes;
- deployment and migration plan;
- rollback plan;
- post-delivery verification checklist;
- observability and incident thresholds.

Label all prepared actions `not executed`.

Actually merging, deploying, applying migrations, mutating infrastructure/data, or changing credentials/permissions requires explicit authorization naming the action and target.

## Approval check

Before an action, resolve:

- exact command/action;
- repository, branch, environment, resource, or dataset;
- destructive, irreversible, public, or security consequences;
- reviewed commit/patch;
- prerequisites and credentials;
- rollback/recovery;
- post-action verification;
- authorization source and scope.

Stop when approval is ambiguous, stale, broader assumptions are required, or the target differs.

## Approval request

Before requesting authorization, report:

- completed nodes and commits/patches;
- behavior, API, schema, migration, infrastructure, permission, and data changes;
- commands and verification evidence;
- review status and resolved findings;
- assumptions and remaining risks;
- exact requested action and target;
- rollback and post-action verification plan.

Never bundle unrelated consequential actions into one implied approval.

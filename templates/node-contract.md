# Node Contract: {{NODE_ID}}

## Identity

- Type: {{NODE_TYPE}}
- Owner/agent: {{SPECIALIST_ROLE}}
- Repository ID: {{REPOSITORY_ID}}
- Objective: {{OBJECTIVE}}
- Maximum retries: {{MAX_RETRIES}}
- Concurrency group: {{CONCURRENCY_GROUP}}

## Dependencies

- Depends on: {{DEPENDENCY_IDS}}
- Dependency reasons: {{DEPENDENCY_REASONS}}
- Required artifacts: {{DEPENDENCY_ARTIFACTS}}

## Context

- Relevant specifications: {{SPECIFICATIONS}}
- Relevant decisions: {{DECISIONS}}
- Relevant source paths: {{SOURCE_PATHS}}
- Repository base: {{BASE_REFERENCE}}
- Workspace path: {{WORKSPACE_PATH}}
- Branch: {{BRANCH_NAME}}

## Ownership

- Allowed paths: {{ALLOWED_PATHS}}
- Forbidden paths: {{FORBIDDEN_PATHS}}
- External resources: {{EXTERNAL_RESOURCES}}

## Constraints

{{CONSTRAINTS}}

## Acceptance Criteria

- {{CRITERION_AND_REQUIRED_EVIDENCE}}

## Verification Commands

- `{{COMMAND}}`

## Required Output

- Handoff: {{OUTPUT_ARTIFACT}}
- Commit or patch: {{COMMIT_OR_PATCH_REQUIREMENT_OR_NULL_FOR_READ_ONLY}}

## Execution Rules

1. Inspect repository rules and existing conventions before modifying code.
2. Start only when required dependency artifacts validate.
3. Modify only allowed paths and resources.
4. Do not silently change public interfaces.
5. Do not weaken tests to make them pass.
6. Run every required verification command and record actual evidence.
7. Record interfaces, assumptions, risks, blockers, and follow-up.
8. When repository changes are contracted, commit completed work in a Git worktree or produce the contracted durable patch. For read-only work, leave both null and provide the declared artifact/evidence.
9. Write the required handoff.
10. Stop after the retry limit or when a blocking condition requires external action.

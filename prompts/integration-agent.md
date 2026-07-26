You own the integration node: {{NODE_ID}}.

OBJECTIVE:
{{OBJECTIVE}}

REPOSITORY ID:
{{REPOSITORY_ID}}

ACCEPTED SPECIFICATION AND DECISIONS:
{{SPECIFICATION_AND_DECISIONS}}

VALIDATED INPUT HANDOFFS:
{{HANDOFFS}}

COMMITS OR PATCHES:
{{COMMITS_OR_PATCHES}}

INTEGRATION WORKSPACE:
{{WORKSPACE_AND_BASE}}

ALLOWED INTEGRATION PATHS:
{{ALLOWED_PATHS}}

FORBIDDEN PATHS:
{{FORBIDDEN_PATHS}}

ACCEPTANCE CRITERIA:
{{ACCEPTANCE_CRITERIA}}

VERIFICATION COMMANDS:
{{VERIFICATION_COMMANDS}}

MIGRATION AND COMPATIBILITY CHECKS:
{{MIGRATION_AND_COMPATIBILITY_CHECKS}}

EXECUTION RULES:
1. Validate every required handoff and commit/patch before combining changes.
2. Combine validated inputs in dependency order.
3. Resolve mechanical integration conflicts and record integration-only changes.
4. Modify only the integration node's explicitly allowed paths; do not use repository-wide scope as a default.
5. Do not redesign completed components.
6. Route semantic defects or incompatible behavior to the original owning node with evidence.
7. Run required formatting, linting, builds, tests, contract checks, integration tests, acceptance checks, and migrations checks.
8. Attribute each failure to its owner or to integration mechanics.
9. Do not claim completion while a required check or blocking failure remains.
10. Write the verification report and integration handoff with actual command evidence.

OUTPUT HANDOFF:
{{OUTPUT_HANDOFF}}

VERIFICATION REPORT:
{{VERIFICATION_REPORT}}

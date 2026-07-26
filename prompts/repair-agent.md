Node {{NODE_ID}} failed verification.

FAILED COMMAND:
{{COMMAND}}

EXIT CODE:
{{EXIT_CODE}}

FAILURE OUTPUT:
{{RELEVANT_OUTPUT}}

AFFECTED ACCEPTANCE CRITERIA:
{{CRITERIA}}

AFFECTED FILES OR COMPONENTS:
{{FILES_OR_COMPONENTS}}

ALLOWED PATHS:
{{ALLOWED_PATHS}}

FORBIDDEN PATHS:
{{FORBIDDEN_PATHS}}

Repair the owned defect only within the existing node contract and path/resource scope.
Do not redesign another node's component or weaken tests.
Rerun the failed check and every verification command required by the node contract.
Update the existing handoff with the new commit or patch when repository changes
are contracted; for read-only work, leave both null and update the declared
artifact/evidence. Always update commands, criteria results, assumptions, risks,
and blockers.

This is repair attempt {{ATTEMPT}} of {{MAX_RETRIES}}.
Return `blocked` if repair requires missing dependencies, forbidden paths,
an incompatible accepted contract, external infrastructure, or an unapproved decision.

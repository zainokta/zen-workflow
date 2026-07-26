# Zen Workflow Skill Refactor Map

## Inventory

- Global principles: graph-use gate, persisted source of truth, evidence over prose, minimum context, isolated writers, human approval.
- Responsibilities and phases: inspection, graph design, node execution, validation, repair, integration, review, delivery boundary.
- Schemas: graph, state, node contract, handoff, decision, review, final report.
- State rules: dependency readiness and node transitions.
- Shared rules: context selection, path/workspace concurrency, verification, retry ownership, stopping conditions.
- Prompts: specialist/node, repair; integration and review are currently prose and become explicit prompts.
- Completion: valid node handoffs, passing integration, non-blocking fresh review, persisted artifacts, approval before delivery.

## Section mapping

| Original section or content | Destination | Notes |
|---|---|---|
| Identity and frontmatter | `SKILL.md` | Preserve discovery trigger and graph-orchestrator identity. |
| Original orchestrator overview | `SKILL.md` | Global purpose and conversation-is-not-state invariant. |
| `Start with the gate` graph-use criteria | `SKILL.md`, `workflows/00-intake.md`, `workflows/03-graph-planning.md` | Root keeps when/when-not; phases perform the decision after intake/discovery/specification. |
| Initial repository inspection rule | `workflows/01-discovery.md` | Expanded into technical discovery; implementation remains forbidden. |
| Persisted `.agent/` contract | `SKILL.md`, `references/workflow-state.md` | Root keeps source-of-truth invariant; reference owns layout and recovery. |
| `Design the graph` node types and deliverable rule | `workflows/03-graph-planning.md` | Planning phase owns node decomposition. |
| Node fields: owner, dependencies, context, paths, criteria, commands, output, retry | `workflows/03-graph-planning.md`, `templates/graph.json`, `templates/node-contract.md` | Canonical artifact shapes move to templates. |
| Dependency-edge semantics | `workflows/03-graph-planning.md`, `references/parallel-execution.md` | Edge requires a consumed interface, decision, artifact, commit, or verification result. |
| Path-conflict detection | `workflows/03-graph-planning.md`, `references/parallel-execution.md` | Combine, sequence, or extract shared ownership. |
| Reject broad/untestable nodes | `workflows/03-graph-planning.md` | Preserve graph-quality gate. |
| Worktree isolation rules | `workflows/04-node-execution.md`, `references/parallel-execution.md` | No concurrent writers in one mutable workspace. |
| Node owner execution rules | `workflows/04-node-execution.md`, `prompts/specialist-agent.md` | Prompt contains the complete invocation contract. |
| Minimum sufficient context package | `references/context-management.md` | Shared by planning, execution, integration, and review. |
| Prohibited context | `references/context-management.md` | No parent transcript, hidden reasoning, irrelevant files/logs, or speculative context. |
| Node state transitions | `references/workflow-state.md`, `templates/state.json` | Canonical state machine and state artifact. |
| Dependency unlocking | `workflows/04-node-execution.md`, `references/workflow-state.md` | Requires completed dependencies and validated handoffs. |
| `Validate, do not trust` handoff checks | `workflows/04-node-execution.md`, `templates/handoff.json` | Preserve schema, paths, artifact, commit/patch, commands, criteria, and evidence checks. |
| Completed-handoff invalidity rules | `workflows/04-node-execution.md`, `references/failure-handling.md` | Missing evidence, invalid scope, or unresolved commit/patch prevents completion. |
| Blocked conditions | `workflows/04-node-execution.md`, `references/failure-handling.md` | Distinguish blocked from failed. |
| `Route repairs to the owner` | `references/failure-handling.md`, `prompts/repair-agent.md` | Same owner, bounded attempts, relevant evidence only. |
| Graph stopping conditions | `references/failure-handling.md` | Contradiction, unavailable dependency, approval gate, incompatible contract, blocking baseline, invalid architecture. |
| `Integrate and review` integration rules | `workflows/05-integration.md`, `prompts/integration-agent.md` | Mechanical conflicts stay in integration; semantic defects return to owner. |
| Repository-level verification | `workflows/05-integration.md` | Format, lint, test, build, contracts, integrations, acceptance, migrations. |
| Fresh-context reviewer inputs and dimensions | `workflows/06-review.md`, `references/context-management.md`, `prompts/review-agent.md` | Implementation conclusions remain untrusted. |
| Review statuses and finding ownership | `templates/review.json`, `workflows/06-review.md` | Preserve `approved`, `approved_with_notes`, `changes_required`. |
| Review repair loop | `workflows/06-review.md`, `references/failure-handling.md` | Findings route to owning nodes, then reintegration and fresh review. |
| `Human boundary` | `SKILL.md`, `references/approval-boundaries.md`, `workflows/06-review.md`, `workflows/07-delivery.md` | Delivery remains optional and explicitly authorized. |
| Approval request contents | `workflows/06-review.md`, `templates/final-report.md` | Include changes, evidence, risks, rollback, and requested action. |
| `Quick checks` | `SKILL.md` | Retain as compact global lifecycle gates. |
| `Common failures` | Relevant workflow blocking conditions and shared references | Keep each warning near the phase that prevents it. |
| Artifact reference introduction | Split across `templates/`, `prompts/`, and `references/` | Remove `references/artifacts.md` after all destinations exist. |
| Graph JSON schema/example | `templates/graph.json` | Add workflow metadata, concurrency group, and explicit dependency reasons. |
| Graph validation rules | `workflows/03-graph-planning.md` | Reject duplicates, unknown dependencies, cycles, missing output, scope, or verification. |
| State JSON schema/example | `templates/state.json`, `references/workflow-state.md` | Add phase, decisions, timestamps, blockers, and commit-or-patch. |
| Node task template | `templates/node-contract.md` | Artifact contract separated from invocation wording. |
| Specialist invocation wording | `prompts/specialist-agent.md` | Placeholder-only reusable prompt. |
| Handoff JSON schema/example | `templates/handoff.json` | Preserve statuses and all evidence, interface, risk, and blocker fields; add patch alternative. |
| Handoff validation details | `workflows/04-node-execution.md` | Verify actual diff including deletes and renames. |
| Decision record | `templates/decision-record.md`, `workflows/02-specification.md` | Add alternatives; decisions constrain downstream nodes. |
| Repair prompt | `prompts/repair-agent.md` | Preserve failure evidence, criteria, paths, attempt, and rerun requirement. |
| Review JSON schema/example | `templates/review.json` | Add evidence and component fallback. |
| Final report | `templates/final-report.md` | Preserve objective, nodes, changes, verification, review, assumptions, risks, rollback/delivery, approval. |
| Missing intake lifecycle | `workflows/00-intake.md`, `templates/intake.md`, `templates/open-questions.md` | Added from the approved refactor requirements without weakening later gates. |
| Missing discovery artifacts | `workflows/01-discovery.md`, `templates/context.md` | Add `.agent/context.md` and `.agent/repository-map.json`. |
| Missing accepted specification phase | `workflows/02-specification.md`, `templates/specification.md`, `templates/decision-record.md` | Makes the pre-graph behavior/security/compatibility gate explicit. |
| Missing explicit integration prompt | `prompts/integration-agent.md` | Extracted from existing integration prose. |
| Missing explicit review prompt | `prompts/review-agent.md` | Extracted from existing fresh-review prose. |
| Missing phase/delivery lifecycle | `SKILL.md`, `references/workflow-state.md`, `workflows/07-delivery.md` | Adds optional approval-gated delivery while preserving the no-automatic-action boundary. |

## Planned canonical ownership

- `SKILL.md`: discovery, routing, global invariants, lifecycle gates, overall completion.
- `workflows/`: one independently loadable procedure per phase.
- `references/`: rules reused by two or more phases.
- `templates/`: canonical artifact shapes only.
- `prompts/`: canonical agent invocation instructions only.
- `agents/openai.yaml`: unchanged Codex UI metadata.

No original rule is intentionally dropped. New intake, discovery, specification, and delivery details come from the approved refactor request.

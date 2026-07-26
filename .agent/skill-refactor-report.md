# Zen Workflow Skill Refactor Report

## Result

The monolithic skill is now a phase-routed zen-workflow with canonical shared rules, artifact templates, and agent prompts. The root remains the entry point and global safety contract.

## Files Created

- Workflows:
  - `workflows/00-intake.md`
  - `workflows/01-discovery.md`
  - `workflows/02-specification.md`
  - `workflows/03-graph-planning.md`
  - `workflows/04-node-execution.md`
  - `workflows/05-integration.md`
  - `workflows/06-review.md`
  - `workflows/07-delivery.md`
- References:
  - `references/workflow-state.md`
  - `references/context-management.md`
  - `references/parallel-execution.md`
  - `references/failure-handling.md`
  - `references/approval-boundaries.md`
- Templates:
  - `templates/intake.md`
  - `templates/context.md`
  - `templates/specification.md`
  - `templates/open-questions.md`
  - `templates/decision-record.md`
  - `templates/graph.json`
  - `templates/state.json`
  - `templates/node-contract.md`
  - `templates/handoff.json`
  - `templates/review.json`
  - `templates/final-report.md`
- Prompts:
  - `prompts/specialist-agent.md`
  - `prompts/repair-agent.md`
  - `prompts/integration-agent.md`
  - `prompts/review-agent.md`
- Refactor artifacts:
  - `.agent/skill-refactor-map.md`
  - `.agent/skill-refactor-report.md`

## Files Modified

- `SKILL.md` — replaced the monolithic procedure with a 182-line router, global invariants, phase gates, source-of-truth rules, and completion boundary.

`agents/openai.yaml` remains valid and unchanged.

## Files Removed

- `references/artifacts.md` — its graph/state/handoff/review/report schemas, node/repair prompts, and shared rules now have canonical destinations under `templates/`, `prompts/`, and `references/`.

## Original Sections Mapped

The complete section-level inventory and destination table is in `.agent/skill-refactor-map.md`. It maps:

- purpose, use gate, source of truth, lifecycle gates, and approval boundary to `SKILL.md`;
- inspection, planning, execution, integration, and review procedures to phase workflows;
- state, context, parallelism, failure, and approval rules to shared references;
- graph, state, node, handoff, decision, review, and final report shapes to templates;
- specialist and repair wording to prompts, with explicit integration and review prompts extracted from the original prose.

No original operational or safety rule was intentionally removed. Intake, specification, multi-repository provenance, preparation-only delivery, and explicit phase state were added from the approved refactor requirements.

## Intentionally Retained Duplication

- Root lifecycle gates repeat the shortest safety-critical conditions also owned by phase/reference files.
- Phase workflows repeat approval, isolation, evidence, or owner-routing rules where omission could authorize unsafe execution.
- Status vocabulary appears in `references/workflow-state.md` and the workflow/prompt that consumes it; schemas remain canonical in `templates/`.

## Validation Performed

- Skill structure:
  - `python3 /home/rakei/.codex/skills/.system/skill-creator/scripts/quick_validate.py .`
  - Result: `Skill is valid!`
- `npx skills` discovery:
  - `npx --yes skills add . --list`
  - Result before the project rename: local path validated; found exactly one available skill. Revalidation after renaming is required.
- Relative links and Markdown fences:
  - Node-based recursive check over `SKILL.md`, `workflows/`, `references/`, `templates/`, `prompts/`, and `.agent/`.
  - Result: 26 Markdown files valid before this report was added.
- Required layout and workflow sections:
  - Node-based exact filename and heading check.
  - Result: 8 workflows, 5 references, 11 templates, and 4 prompts present; every workflow has all ten required sections.
- JSON parsing:
  - `JSON.parse` over every `templates/*.json`.
  - Results:
    - `graph.json`: valid
    - `state.json`: valid
    - `handoff.json`: valid
    - `review.json`: valid
- Lifecycle/safety assertions:
  - Node-based assertions for phase order, root line target, conversation-is-not-state, all integration dependencies, node/review statuses, blocked recovery, workspace/external-resource isolation, evidence, repository provenance, integration/review artifacts, and preparation/performance approval.
  - Result: valid; `SKILL.md` is 182 lines.
- Stale-reference search:
  - Result: only the intentional historical `references/artifacts.md` removal entry in the refactor map and the desired “Do not preload every workflow” rule.
- Forward test:
  - Complex OAuth scenario correctly routed phase by phase; a one-string/one-command change exited to a focused loop.
  - Initial contract gaps were repaired and the final Phase 7 preparation/performance routing recheck passed.
- Independent read-only review:
  - Initial findings were repaired.
  - Final result: no unresolved Critical or Important findings; contract-ready.

## Unresolved Issues

- The workspace is not a Git repository, so Git diff, commit, and push validation cannot run here. This does not affect local Agent Skills or `npx skills` discovery validation.
- No application runtime was added; this remains documentation/workflow orchestration as required.

## Manual Review Candidates

- `SKILL.md` — routing and global lifecycle gates.
- `references/workflow-state.md` — phase/node/handoff/review state semantics and recovery.
- `templates/graph.json` and `templates/state.json` — repository, workspace, concurrency, and provenance fields.
- `references/approval-boundaries.md` and `workflows/07-delivery.md` — preparation versus performance authorization.

## Future Improvements

Add a deterministic graph/handoff validator only when real workflow usage shows manual artifact validation is insufficient. No runtime or dependency is justified by this documentation-only refactor.

## Unrelated Files

The pre-existing `codedb.snapshot` was not modified.

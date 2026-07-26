# Context Management

Give each phase or agent the minimum sufficient context needed to produce and verify its owned artifact.

## Context package

For an implementation node, compose:

```text
repository rules
+ specialist instructions
+ complete node contract
+ relevant accepted specification excerpts
+ relevant source files and existing patterns
+ affected decisions
+ validated dependency handoffs
+ current branch/worktree state
+ acceptance criteria and verification commands
+ required output schema
```

For a phase, load the root router, the current workflow, its named references, its required templates, and its declared inputs. Do not preload every workflow.

## Include

- Complete node contract or current phase contract.
- Exact accepted criteria and constraints that affect the work.
- Relevant source paths and adjacent interfaces.
- Dependency handoffs rather than dependency conversations.
- Decisions whose affected phases/nodes include the current owner.
- Focused command evidence and concise environment prerequisites.

## Exclude

- Full parent conversation history.
- Hidden reasoning or another agent's conclusions as authority.
- Unrelated repository files, specifications, nodes, or outputs.
- Duplicate copies of large specifications.
- Speculative information not recorded as an assumption.
- Raw large logs when a relevant excerpt plus durable full-log path suffices.

## Repository retrieval

Start from repository instructions and the paths/artifacts named by the current contract. Retrieve more only when code evidence, a dependency, an error, or an acceptance criterion identifies a concrete need. Prefer existing architecture/index tools and repository conventions before broad text search.

Do not ask the user for facts that can be discovered reliably from the repository. Ask when the answer is not discoverable and a wrong assumption materially changes behavior, security, compatibility, storage, ownership, architecture, or approval.

## Large inputs

- Specifications: cite relevant sections; keep one canonical complete artifact.
- Logs: preserve the full output durably when useful, forward only command, exit code, failure locus, and relevant excerpt.
- Diffs: give reviewers the final complete diff; give repair owners the affected files/hunks plus required surrounding context.
- Multi-repository work: state repository, base, branch/worktree, and artifact provenance explicitly.

If a node needs nearly the whole repository or full workflow history, reconsider its boundary before increasing its context budget.

## Fresh review

The reviewer receives:

- original objective;
- accepted specification and relevant decisions;
- graph summary;
- final integration diff;
- verification report;
- node and integration handoffs;
- assumptions and remaining risks.

Do not provide implementation reasoning or conclusions as authoritative. Review findings must be derived from the accepted contract, final code/artifacts, and evidence.

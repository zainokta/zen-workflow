# Zen Workflow

A small escalation workflow for engineering tasks that genuinely benefit from a dependency graph.

Default to one focused implementation loop. Use graph mode only for multi-repository work, at least three independent non-overlapping deliverables, materially useful specialist parallelism, or high-risk work needing isolated review.

## Workflow

```text
Engineering objective
└─ Inspect repository, real call flow, tests, and constraints
   └─ Does graph mode clearly help?
      ├─ No: focused loop
      │  └─ root cause → smallest change → focused check → repository check → diff review
      └─ Yes: graph mode
         └─ spec → dependency graph → ready nodes → validated handoffs
            → integration → fresh review when justified → final report
```

### Focused loop — default

Use one agent and create no workflow artifacts:

1. Trace the real flow end to end.
2. Make the smallest root-cause change.
3. Run the narrowest meaningful check and required repository-level checks.
4. Inspect the final diff and report behavior, evidence, risks, and approval-gated actions.

### Graph mode — escalation

Use graph mode only when its coordination cost is justified. Persist only:

```text
.agent/
├── spec.md
├── graph.json
├── handoffs/<node-id>.json
└── verification.md
```

One orchestrator owns `graph.json`. Writers use isolated worktrees. Parallel nodes cannot overlap paths or shared external resources. Dependents unlock only from inspected handoffs. Integration and fresh review are added when the work justifies them.

Zen Workflow handles routing and orchestration. Keep using your agent's native planning, debugging, testing, Git, and review capabilities; this skill does not duplicate them.

## Install for humans

### Hermes Agent

Review [SKILL.md](SKILL.md), then install and verify:

```bash
HERMES_HOME="${HERMES_HOME:-$HOME/.hermes}"
mkdir -p "$HERMES_HOME/skills/zen-workflow"
curl -fsSL https://raw.githubusercontent.com/zainokta/zen-workflow/master/SKILL.md \
  -o "$HERMES_HOME/skills/zen-workflow/SKILL.md"
hermes skills list
```

Start a new session or run `/reload-skills`, then invoke it explicitly when needed:

```text
Use $zen-workflow for this engineering objective. Choose focused or graph mode using its gate, then execute and verify the work.
```

### Other agents

Use the agent's native skill installer when available. Install this repository's `SKILL.md` under the name `zen-workflow`, reload skills, and verify that the skill appears in the agent's skill list.

If the agent has no installer, place the file at:

```text
<agent-user-skills-directory>/zen-workflow/SKILL.md
```

Use the user-scoped skills directory documented by that agent; do not guess a global path.

## Install by asking an AI agent

Send this instruction to an agent that can manage its own skills:

```text
Install the skill from:
https://raw.githubusercontent.com/zainokta/zen-workflow/master/SKILL.md

Use your native skill installer if available. Otherwise place only SKILL.md at
<your user skills directory>/zen-workflow/SKILL.md. Verify its frontmatter name
is zen-workflow, reload skills if required, and report the installed path and
verification result. Do not modify unrelated agent configuration.
```

## Usage

```text
Use $zen-workflow for this engineering objective. Choose focused or graph mode using its gate, then execute and verify the work.
```

See [SKILL.md](SKILL.md) for the complete operating contract.

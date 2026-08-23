# Skills

Nico Ritschel's collection of reusable [Agent Skills](https://agentskills.io) for coding agents.

## Available Skills

| Skill | Description |
|-------|-------------|
| [workflow](plugins/nicosuave-skills/skills/workflow/) | Select and compose multi-agent workflow topologies for complex coding, research, investigation, review, and implementation tasks. |
| [context-engineering](plugins/nicosuave-skills/skills/context-engineering/) | Design and audit agent instructions, scoped context, pointers, task packets, handoffs, and completion criteria without bloating always-loaded context. |
| [primary-source-research](plugins/nicosuave-skills/skills/primary-source-research/) | Research current or precise claims from the sources that directly own them, preserving scope, provenance, and the distinction between fact and inference. |

## Installation

Use one installation method per agent to avoid duplicate skill entries.

### Codex

```bash
codex plugin marketplace add nicosuave/skills
codex plugin add nicosuave-skills@nicosuave
```

### Claude Code

```bash
claude plugin marketplace add nicosuave/skills
claude plugin install nicosuave-skills@nicosuave
```

### Agent Skills

Use the standalone installer for other compatible agents or to install individual skills:

```bash
bunx skills add nicosuave/skills
```

Install one skill:

```bash
bunx skills add nicosuave/skills --skill workflow
bunx skills add nicosuave/skills --skill context-engineering
bunx skills add nicosuave/skills --skill primary-source-research
```

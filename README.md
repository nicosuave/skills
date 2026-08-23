# Skills

Nico Ritschel's collection of reusable [Agent Skills](https://agentskills.io) for coding agents.

## Available Skills

| Skill | Description |
|-------|-------------|
| [workflow](plugins/nicosuave-skills/skills/workflow/) | Select and compose multi-agent workflow topologies for complex coding, research, investigation, review, and implementation tasks. |
| [context-engineering](plugins/nicosuave-skills/skills/context-engineering/) | Design and audit agent instructions, scoped context, pointers, task packets, handoffs, and completion criteria without bloating always-loaded context. |
| [primary-source-research](plugins/nicosuave-skills/skills/primary-source-research/) | Research current or precise claims from the sources that directly own them, preserving scope, provenance, and the distinction between fact and inference. |

## Installation

Choose either the regular Agent Skills installation or the native plugin installation. Installing both exposes duplicate copies of the same skills.

### Agent Skills

Install the skills as ordinary files for Codex, Claude Code, or another compatible agent:

```bash
bunx skills add nicosuave/skills
```

Install one skill:

```bash
bunx skills add nicosuave/skills --skill workflow
bunx skills add nicosuave/skills --skill context-engineering
bunx skills add nicosuave/skills --skill primary-source-research
```

### Claude Code plugin

```bash
claude plugin marketplace add nicosuave/skills
claude plugin install nicosuave-skills@nicosuave
```

### Codex plugin

```bash
codex plugin marketplace add nicosuave/skills
codex plugin add nicosuave-skills@nicosuave
```

Plugin-installed skills use the `nicosuave-skills` namespace, such as `nicosuave-skills:workflow`.

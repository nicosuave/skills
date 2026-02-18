# Skills

A collection of [Agent Skills](https://agentskills.io) tested with Claude Code and Codex. Should work with any agent that supports subagent spawning, but untested elsewhere.

## Available Skills

| Skill | Description |
|-------|-------------|
| [multiclaude](multiclaude/) | Multi-phase parallel agent orchestration for deep investigations |

## Installation

```bash
npx skills add nicosuave/skills
# or
bunx skills add nicosuave/skills
```

Install a specific skill:

```bash
npx skills add nicosuave/skills/multiclaude
```

## Codex Note

Skills that write to `~/.agents/findings/` require adding it as a writable root:

```toml
# ~/.codex/config.toml
[sandbox_workspace_write]
writable_roots = ["~/.agents/findings"]
```

Or pass `--add-dir ~/.agents/findings` at invocation.

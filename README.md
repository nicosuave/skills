# Workflow

A composable multi-agent orchestration skill for coding/research harnesses such as Claude Code and Codex.

This replaces a rigid `explore → fan-out → synthesize` workflow with a small set of orchestration patterns selected at runtime:

- chain
- fan-out
- scout → fan-out
- ensemble
- critic loop
- wavefront / DAG
- work queue / team

The core idea is simple:

> Choose the simplest topology that creates useful independence. Parallelize independent uncertainty, not dependency chains. Adapt the topology as evidence arrives.

## Layout

```text
workflow/
├── SKILL.md
└── references/
    ├── patterns.md
    ├── claude-code.md
    ├── codex.md
    ├── research.md
    └── examples.md
```

`SKILL.md` is intentionally compact enough to load as the controlling policy. The reference files contain deeper topology guidance and harness-specific mechanics so the conceptual policy does not become brittle as runtimes evolve.

## Design changes from the old multiclaude skill

The previous skill encoded one useful topology as a mandatory protocol:

```text
scout → user confirmation → fan-out → synthesizer
```

This version instead:

- treats scout → fan-out as one pattern among several;
- does not require user confirmation merely to delegate work;
- separates topology from context propagation, communication, verification, isolation, and stopping policy;
- prefers dynamic routing based on task dependency structure;
- supports both hierarchical subagents and richer team/workflow runtimes;
- does not require every worker to write a findings file;
- does not require a separate synthesizer;
- does not require the strongest model for every worker;
- expands agent count only when additional independent work is likely to pay for its coordination cost.

## Installation

Copy `workflow/` into the skill directory used by your harness, or install the repository using your normal Agent Skills mechanism.

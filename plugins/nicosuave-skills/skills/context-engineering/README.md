# Context Engineering

A composable skill for designing and auditing agent context across `AGENTS.md`, `CLAUDE.md`, skills, reference documents, task packets, handoffs, and durable artifacts.

The core idea is:

> Put each fact, instruction, and artifact in the narrowest context that reliably reaches every consumer that needs it, and no others.

It combines:

- conditional context pointers;
- progressive disclosure;
- canonical sources of truth;
- bounded subagent and handoff packets;
- observable completion criteria;
- lightweight decision state;
- pruning of duplication, stale caches, no-ops, and sediment.

## Why this is both a skill and an `AGENTS.md` rule

The small, always-applicable kernel belongs in `AGENTS.md`: keep universal instructions compact, use scoped docs and conditional pointers, avoid duplication, and give workers bounded context.

The full audit and refactoring process does **not** belong in always-loaded context. This skill is reached only when context artifacts themselves need to be designed, debugged, or rewritten.

## Layout

```text
context-engineering/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── examples.md
```

## Installation

Copy `context-engineering/` into the skill directory used by the harness, or add it to the existing `nicosuave/skills` repository.

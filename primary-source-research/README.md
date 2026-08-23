# Primary Source Research

A reusable evidence policy for technical, scientific, product, policy, legal, financial, and market research.

The core idea is:

> Trace each material claim to the closest source with authority to establish it. Preserve version, date, scope, and provenance. Separate what the source says from what you infer.

The skill deliberately does not mandate one research topology or a background agent. It composes with the existing `workflow` skill when separate evidence facets benefit from independent or parallel investigation.

## Layout

```text
primary-source-research/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── source-owners.md
```

`SKILL.md` contains the controlling research policy. The reference file contains domain-specific source hierarchies so they do not burden every research run.

## Installation

Copy `primary-source-research/` into the skill directory used by the harness, or add it to the existing `nicosuave/skills` repository.

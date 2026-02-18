---
name: multiclaude
description: Multi-phase agent orchestration. Fan out parallel agents to explore, research, and synthesize findings. Use when the user wants comprehensive investigation of a problem, codebase area, or question that benefits from parallel exploration.
allowed-tools: Read Write(~/.agents/findings/*) Bash(mkdir:~/.agents/findings)
---

# Parallel Agent Orchestration

You are orchestrating a multi-phase investigation. Follow the pattern below precisely.

## Platform Notes

How you spawn sub-agents depends on your platform:

- **Claude Code**: Use the Task tool with `subagent_type=general-purpose, model=opus`. Spawn parallel agents by issuing multiple Task calls in a single message. Always use the strongest available model (opus).
- **Codex**: Use the sandbox execution model to run parallel investigations. Spawn sub-tasks and ensure each writes its findings to disk.
- **Other agents**: Use whatever sub-agent or task spawning mechanism your platform provides. The key requirements are: (1) agents can read/write files, (2) multiple agents can run in parallel, (3) each agent gets its own isolated prompt with context you provide.

## Findings Directory

All findings go in `~/.agents/findings/`. Generate a session slug using the adjective-verb-noun pattern (e.g., `curious-mapping-auth`). Each file in the session follows this naming:

- Main orchestrator file: `{slug}.md`
- Agent findings: `{slug}-agent-{agent-id}.md`

The orchestrator file (`{slug}.md`) tracks the overall mission, phase transitions, and final synthesis. Agent files contain each agent's raw findings.

## Phase 1: Explore & Inventory (Single Agent)

Spawn ONE agent to comprehensively explore the problem space. Its prompt must include:

1. The user's original question/problem
2. Instruction to be thorough: read whole files, trace call chains, check configs
3. Instruction to write findings to `~/.agents/findings/{slug}-agent-{agent-id}.md` as structured markdown
4. Instruction to end its findings with a `## Recommended Facets` section listing 2-4 independent sub-investigations that would benefit from parallel exploration
5. **CRITICAL**: Instruction to return a list of ALL findings files it created, as the LAST thing in its response, in this exact format:
   ```
   FINDINGS_FILES:
   - /path/to/file1.md
   - /path/to/file2.md
   ```

After this agent returns, write the orchestrator file (`{slug}.md`) with:
- The mission statement
- Phase 1 summary
- The facets identified

Show the user the facets and ask if they want to adjust before fanning out.

## Phase 2: Fan Out (2-4 Parallel Agents)

Spawn 2-4 agents IN PARALLEL. Each agent:

- Gets ONE facet to investigate deeply
- Must write findings to `~/.agents/findings/{slug}-agent-{agent-id}.md`
- Must be told what the OTHER agents are investigating (so it avoids overlap)
- Must be told the Phase 1 context (paste the explore agent's summary, not the full file)
- **CRITICAL**: Must return `FINDINGS_FILES:` list as the last thing in its response

## Phase 3: Synthesize (Single Agent)

Spawn ONE agent that:

- Reads ALL findings files from phases 1 and 2
- Looks for: contradictions, gaps, connections, patterns, surprises
- Writes synthesis to `~/.agents/findings/{slug}-synthesis.md`
- Ends with `## Open Questions` (things still unclear) and `## Recommendations` (what to do next)
- **CRITICAL**: Returns `FINDINGS_FILES:` list

Update the orchestrator file with the synthesis summary.

## Phase 4: Optional Second Fan-Out

If the synthesis reveals follow-up work worth parallelizing, repeat Phase 2-3 with new facets. Only do this if the user approves or the original request clearly warrants it.

## Rules

1. **Use the strongest model available.** Always prefer the most capable model your platform offers for every phase.
2. **Always return FINDINGS_FILES.** Every agent prompt must include this instruction. Parse the returned list and track all files.
3. **Never skip the confirmation** between Phase 1 and Phase 2. Show the facets, let the user adjust.
4. **Agents must write files, not just return text.** The findings files ARE the artifact. Return messages are ephemeral; files persist.
5. **Include context in every agent prompt.** Agents start fresh. Give them the mission, relevant prior findings, and their specific assignment.
6. **Update the orchestrator file** (`{slug}.md`) after every phase with a running log of what happened, which agents ran, and what files were produced.
7. **Final response** to the user should summarize the key findings and list all files produced.

## Slug Generation

Generate a slug like: `{adjective}-{verb}-{noun}` where:
- adjective: a descriptive word (curious, bright, fuzzy, etc.)
- verb: an -ing word (mapping, tracing, hunting, etc.)
- noun: related to the investigation topic when possible

Examples: `curious-mapping-auth`, `bright-tracing-perf`, `fuzzy-hunting-memory`

## Orchestrator File Template

```markdown
# Investigation: {mission title}

## Mission
{user's original request}

## Session
- Slug: {slug}
- Started: {timestamp}
- Status: {in-progress|complete}

## Phase 1: Explore
- Agent: {agent-id}
- File: {path}
- Summary: {2-3 sentences}
- Facets identified:
  1. {facet}
  2. {facet}
  ...

## Phase 2: Fan Out
| Facet | Agent | File | Key Findings |
|-------|-------|------|-------------|
| ... | ... | ... | ... |

## Phase 3: Synthesis
- File: {path}
- Key findings: ...
- Open questions: ...
- Recommendations: ...

## All Files
- {list every findings file produced}
```

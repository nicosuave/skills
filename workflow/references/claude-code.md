# Claude Code Mapping

Use the orchestration policy in `SKILL.md`, then select the cheapest Claude Code primitive capable of expressing it.

Claude Code currently exposes several qualitatively different coordination layers. They should not be treated as interchangeable.

## 1. Ordinary subagents

Use ordinary subagents for small, bounded delegation:

- one-off repository inspection;
- package-local investigation;
- independent review;
- a narrow implementation task;
- a small fan-out.

The lead remains the primary coordinator.

Good mappings:

```text
chain
fan-out
scout → fan-out
ensemble
small wavefront
critic loop
```

For ensembles, preserve independence by giving each subagent only the shared problem statement and necessary evidence, not sibling reasoning.

## 2. Agent Teams

Prefer Agent Teams when the work benefits from a persistent team rather than parent → worker RPC.

Useful capabilities include:

- independent teammate contexts;
- shared tasks;
- task dependencies;
- self-claiming of available work;
- direct teammate communication;
- a lead coordinating progress.

This makes teams particularly appropriate for:

```text
work queues
dynamic DAGs
large independent implementation partitions
competing hypotheses with cross-critique
long-running investigations whose decomposition evolves
```

Do not force Agent Teams for sequential work. If most tasks are blocked on one another, a chain or lead-managed wavefront is cheaper and clearer.

Do not require an extra user confirmation before forming a team merely because the old multiclaude skill did so. Use the normal Claude Code permission/approval model.

### Communication policy

Avoid all-to-all chatter.

Prefer:

- shared task state for coordination;
- direct teammate messages only when one teammate has information another actually needs;
- lead synthesis at barriers;
- artifacts for large reusable results.

The goal is sparse useful communication, not simulated meetings.

## 3. Dynamic Workflows

When available, prefer Dynamic Workflows for high-width, repeatable, or algorithmically controlled orchestration.

A workflow script is often better than an LLM lead for:

- hundreds of files;
- repeated map/reduce analysis;
- deterministic loops;
- migration batches;
- retry-until-check-passes flows;
- scripted branching;
- large-scale independent audits.

Conceptually:

```text
LLM lead:
  decides what should run next

workflow script:
  code decides what should run next
```

Use the workflow runtime when coordination logic can be stated more reliably as code than repeatedly inferred by an LLM.

Examples:

### Repository-wide migration

```text
enumerate files
→ filter relevant files
→ spawn bounded migration worker per shard
→ join
→ run global verifier
→ retry failed shards only
```

### Research search loop

```text
seed queries
→ parallel search
→ score coverage
→ generate queries for uncovered facets
→ repeat until coverage threshold/no-new-evidence
→ synthesize
```

### Deterministic reviewer loop

```text
worker changes module
→ run test/check
→ if fail: send exact diagnostics back
→ retry with cap
```

## 4. Choosing among Claude primitives

Use roughly this hierarchy:

```text
small bounded task
  → ordinary subagent

adaptive team with shared tasks / peer communication
  → Agent Teams

high-width or repeatable graph whose control flow is code-like
  → Dynamic Workflows
```

These can compose.

For example:

```text
workflow script
  → spawns several bounded analysis jobs
  → result identifies ambiguous subsystem
  → Agent Team handles adaptive subsystem investigation
  → script resumes deterministic verification
```

Do not invent Agent Team or Dynamic Workflow semantics if the current Claude Code build does not expose them. Fall back to lead-managed subagents and a wavefront.

## 5. Context policy

Prefer fresh contexts for:

- ensembles;
- independent code review;
- competing hypotheses.

Prefer bounded task packets for:

- module reviews;
- implementation partitions;
- research facets.

Prefer fuller inherited context only when the teammate truly continues the lead's reasoning and the extra history is likely to matter.

## 6. Edits and ownership

For parallel implementation:

- assign disjoint files/modules where possible;
- use worktrees/sandboxes if the harness provides them and overlapping repository state would be risky;
- avoid letting several teammates rewrite the same central file concurrently.

If the change is strongly cross-cutting, use a chain or wavefront instead of forcing file-level parallelism.

## 7. Practical routing examples

### "Deep review this repository"

```text
lead
→ direct fan-out:
   - architecture/correctness
   - security
   - performance
   - tests/operability
→ integrate
→ targeted follow-up only where findings justify it
```

### "Figure out why this flaky failure happens"

```text
ensemble:
  - race hypothesis
  - environment/state hypothesis
  - test-order hypothesis
→ judge
→ focused reproduction chain
→ patch
→ independent verifier
```

### "Migrate 600 files"

Prefer Dynamic Workflow if available:

```text
discover files
→ group by migration shape
→ parallel workers
→ mechanical checks
→ failed-only repair loop
→ global tests
```

### "Implement a feature touching 4 independent packages"

```text
planner
→ parallel package owners
→ integration owner
→ verifier
```

If package boundaries are uncertain, insert a scout/planner first.

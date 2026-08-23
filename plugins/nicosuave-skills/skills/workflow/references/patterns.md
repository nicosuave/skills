# Orchestration Pattern Catalog

This file expands the pattern shorthand used by `SKILL.md`.

## Chain

```text
A → B → C
```

A chain is correct when each stage transforms state that the next stage genuinely depends on.

Good examples:

```text
reproduce bug
  → isolate mechanism
  → design fix
  → implement fix
  → run targeted verification
```

```text
inventory legacy API
  → derive compatibility requirements
  → design new interface
  → migrate callers
```

A common mistake is parallelizing stages whose work cannot be usefully started until an upstream decision lands. That spends tokens to manufacture speculative branches which later get discarded.

### Same agent vs fresh agent

Use the same agent when:

- local codebase state and partial reasoning are expensive to reconstruct;
- the task is an uninterrupted debugging trace;
- anchoring is not a major risk.

Use a fresh agent when:

- you want independent validation;
- upstream work may contain a subtle assumption;
- the downstream role is semantically different, such as reviewer or judge.

Use bounded context when you want continuity without copying the entire transcript.

---

## Fan-out

```text
        ┌─ facet A ─┐
leader ─┼─ facet B ─┼→ integrate
        └─ facet C ─┘
```

Fan-out works when:

- partitions are independent;
- boundaries are easy to state;
- duplicate work can be avoided;
- integration is cheaper than serial execution.

Strong partition axes include:

- package/module ownership;
- independent questions;
- orthogonal quality dimensions;
- disjoint data sources;
- separate APIs or services.

Weak partition axes include vague roleplay such as:

- "optimist";
- "pessimist";
- "creative thinker";

unless disagreement itself is the objective.

### Fan-out integration

The leader should integrate directly unless a fresh synthesizer provides useful independence.

A worker should generally return:

```text
- findings / result
- evidence
- uncertainties
- any new task worth creating
```

Do not dump unfiltered tool traces into the leader.

---

## Scout → Fan-out

```text
scout → decomposition → [A B C] → integrate
```

Use when the task has hidden structure.

The scout is not a miniature full investigation. Its job is to produce a better task graph.

A useful scout output includes:

```text
problem map
dependency boundaries
high-value facets
shared context needed by downstream workers
likely conflicts / overlap
suggested ordering constraints
```

The leader may convert the proposed graph into:

- direct fan-out;
- a wavefront;
- a chain;
- an ensemble;
- no delegation at all.

The scout's recommendation is advisory.

---

## Ensemble

```text
candidate A ─┐
candidate B ─┼→ judge
candidate C ─┘
```

Ensembles are about **independence**, not partitioning.

Good uses:

- architecture selection;
- root-cause analysis under ambiguity;
- independent threat models;
- interpretation of underspecified behavior;
- generating several plausible migration plans.

### Preserve variance

For the first pass:

- use fresh or minimally shared context;
- do not reveal sibling answers;
- do not force everyone through the same reasoning template;
- ask for evidence and falsifiable claims.

Then reconcile.

### Reconciliation methods

#### Judge

A fresh or leader judge compares answers and selects/merges them.

Default option.

#### Cross-critique

Candidates inspect other candidates, then a judge resolves.

Use only when initial disagreement is meaningful.

#### Consensus

Useful only where agreement is a reasonable proxy for correctness. Do not use consensus to paper over correlated model errors.

---

## Critic Loop

```text
producer → evaluator → revision → evaluator ...
```

Use when quality can improve through targeted feedback.

The evaluator should return concrete deltas, not generic prose.

Bad:

```text
This could be more robust.
```

Good:

```text
Fails when `foo` is nil because path X bypasses guard Y.
Add a regression test for condition Z.
```

### Stop conditions

At least one should be present:

```text
tests pass
zero material review findings
metric threshold reached
no meaningful delta from previous round
max rounds reached
```

Two or three rounds are usually more than enough unless the loop is anchored to deterministic tests.

---

## Wavefront / DAG

```text
[A B] → C → [D E F] → G
```

This should be the default mental model for complex work.

A DAG allows you to express:

- independent evidence gathering;
- merge points;
- decisions;
- parallel implementation;
- verification;
- targeted repair.

### Barrier discipline

Do not wait on unrelated workers.

If `D` depends only on `A`, it may start as soon as `A` is done even if `B` is still running.

Likewise, if one slow worker is not required for the final decision, use a join condition such as "enough" rather than blindly waiting for all.

### Dynamic DAGs

Workers may reveal new tasks.

Spawn those only when they add distinct value.

Examples:

```text
security scout finds custom crypto
→ create targeted crypto review
```

```text
migration worker finds generated code
→ skip manual migration for that subtree
```

---

## Work Queue / Team

```text
task graph:
  ready tasks → workers self-claim
  completion → unlock dependents
  discoveries → append tasks
```

This is best when the runtime has shared coordination.

Useful characteristics:

- many items;
- similar task shapes;
- explicit ownership;
- bounded side effects;
- dependencies can be represented as tasks;
- workers can make progress independently.

Avoid decentralized queues for tightly coupled refactors where workers constantly need to negotiate overlapping edits.

---

# Composition

Patterns are meant to nest.

## Scout + Wavefront + Critic

```text
scout
  → [architecture inventory, test inventory, API inventory]
  → planner
  → [implementation A, implementation B]
  → verifier
  → targeted repair
```

## Ensemble + Chain

```text
[independent hypotheses]
  → judge selects most likely mechanism
  → reproduce
  → patch
  → verify
```

## Fan-out + Local Critic Loops

```text
package A worker → tests/reviewer ↺
package B worker → tests/reviewer ↺
package C worker → tests/reviewer ↺
           ↓
      integration test
```

## Work Queue + Final Fresh Judge

```text
many migration tasks
  → shared queue
  → workers
  → fresh repository-wide verifier
```

---

# Choosing by Task Shape

| Task shape | Default pattern |
|---|---|
| strict dependency chain | chain |
| known independent facets | fan-out |
| unknown decomposition | scout → fan-out |
| competing explanations | ensemble |
| objective quality loop | critic loop |
| mixed dependencies | wavefront / DAG |
| many dynamic tasks + shared coordination | work queue |
| broad research | scout → fan-out or wavefront |
| repository review | fan-out by quality dimension or module |
| implementation across disjoint modules | fan-out / wavefront |
| cross-cutting refactor | chain or wavefront with strong ownership |
| flaky bug | ensemble hypotheses → chain |

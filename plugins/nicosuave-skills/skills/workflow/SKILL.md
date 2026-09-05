---
name: workflow
description: "Use when the user asks to design agent orchestration or concrete independent subtasks justify delegation and need coordination. Not for ordinary multi-step work, quick searches, or tasks that are merely large or uncertain. Choose the smallest useful workflow, including staying single-agent."
---

# Workflow

Start with the current agent. Use multiple agents only when a concrete independent assignment is likely to save enough time or improve confidence to outweigh briefing, duplicated context, coordination, and review. The patterns below are options, not stages to execute or a reason to create more agents.

The governing principle is:

> Choose the simplest orchestration topology that creates useful independence. Parallelize independent uncertainty, not dependency chains. Adapt the topology as evidence arrives. Communicate minimally. Verify at boundaries.

Do not require user confirmation merely to spawn or fan out agents. Ask the user only when their judgment is genuinely needed to resolve product intent, scope, risk, or an irreversible choice, or when the harness itself requires approval for an action.

## 1. Decide Whether To Delegate

Before spawning agents, classify the task.

Reasons to stay single-agent include:

- the next useful step is obvious;
- later work depends strongly on earlier work;
- the task is small enough that coordination overhead dominates;
- a single context benefits from uninterrupted local reasoning;
- independent answers would not materially improve confidence;
- agents would contend on the same files or state.

Consider delegation when the following identify a specific useful assignment; no item is sufficient without the cost/benefit check above:

- there are independent facets that can be investigated concurrently;
- several competing hypotheses or designs should be explored without anchoring;
- broad search/review can be partitioned cleanly;
- implementation can be split by ownership boundaries;
- an independent verifier/reviewer materially improves correctness;
- the task contains a large number of similarly shaped items;
- existing evidence identifies a distinct unresolved question another agent can answer independently.

Complexity or parallelizable uncertainty alone is not enough. Before spawning, know what the worker will return, what useful work continues locally, and why reconciling the result is cheaper or more reliable than doing it locally. If the next step is a quick read or lookup, do it locally first.

## 2. Select a Topology

Choose the topology from the dependency structure, not from habit.

### Chain

Shape:

```text
A → B → C
```

Use when later stages consume the output of earlier stages.

Typical uses:

- reproduce → localize → patch → verify;
- investigate → design → implement;
- extract facts → analyze → write;
- migrate schema → migrate code → run integration tests.

Prefer the same agent across adjacent stages when accumulated context is unusually valuable. Prefer a fresh downstream agent when independence or de-anchoring is more valuable.

### Fan-out

Shape:

```text
        ┌→ A ─┐
leader ─┼→ B ─┼→ leader
        └→ C ─┘
```

Use when the decomposition is already obvious and facets are substantially independent.

Examples:

- security / correctness / performance / tests;
- independent packages or services;
- several unrelated research questions;
- file ownership split across modules.

Do not insert a scout merely because the workflow contains parallelism.

### Scout → Fan-out

Shape:

```text
scout → discovered facets → parallel workers → leader
```

Use when useful parallel work probably exists but the correct decomposition is not yet known.

The scout should:

- inventory the problem space;
- identify dependency boundaries;
- propose only facets with genuinely distinct work;
- return enough context for the leader to route the next wave.

The leader may fan out immediately. There is no mandatory user checkpoint.

If new evidence reveals a valuable branch, add another wave. Do not force a predetermined number of waves.

### Ensemble

Shape:

```text
A ─┐
B ─┼→ judge
C ─┘
```

Give multiple agents the same or nearly the same target with intentionally independent contexts.

Use for:

- competing root-cause hypotheses;
- architecture alternatives;
- independent security or correctness reviews;
- ambiguous specifications;
- situations where model variance is itself useful evidence.

Preserve independence. Do not feed one candidate's reasoning to another before the initial answers are produced unless cross-critique is explicitly useful.

Default to a single reconciliation/judge step. Add cross-critique only when disagreement is material enough to justify the extra communication.

### Critic Loop

Shape:

```text
produce → evaluate → revise ↺
```

Use when output quality can be checked against a meaningful criterion.

Suitable criteria include:

- tests;
- benchmark targets;
- compiler/type checker output;
- security invariants;
- acceptance criteria;
- a focused independent review.

Stop when:

- the criterion passes;
- the reviewer has no material findings;
- another round is unlikely to change the result;
- two consecutive rounds make no meaningful progress;
- the configured effort budget is exhausted.

Do not create an endless reviewer-worker conversation.

### Wavefront / DAG

Shape:

```text
[A B C] → D → [E F] → G
```

Use for tasks that mix independent and dependent work.

Use this only when already-justified parallel assignments have real dependency barriers. Ordinary multi-step investigations and implementations can stay with one agent.

Examples:

- parallel scouts → planner → parallel implementation owners → verifier;
- schema analysis + API analysis → migration design → package-specific changes;
- independent evidence gathering → hypothesis selection → targeted validation.

Treat the graph as dynamic. New evidence may create, remove, or reorder nodes.

### Work Queue / Team

Shape:

```text
shared task graph
workers claim unblocked work
tasks create additional tasks
```

Use only when the current harness provides durable shared coordination such as:

- a shared task list;
- dependency-aware task claiming;
- peer-to-peer messaging;
- a script/workflow runtime capable of spawning jobs dynamically.

Best for:

- many similarly shaped work items;
- large migrations;
- repository-wide audits;
- queues where workers can discover additional tasks;
- workloads where centralized assignment becomes unnecessary overhead.

If the harness lacks shared coordination primitives, emulate this with a leader-managed wavefront rather than pretending a decentralized queue exists.

## 3. Keep Orthogonal Concerns Separate

Topology is only one dimension. Select these independently.

### Decomposition

- `fixed`: facets are known before execution;
- `leader_discovered`: a scout/leader determines facets;
- `emergent`: workers or runtime evidence create new tasks.

### Scheduling

- `sequential`;
- `parallel`;
- `dag`;
- `work_queue`.

### Context

- `fresh`: no parent conversation history unless explicitly supplied;
- `bounded`: only a summary/artifact or selected recent context;
- `full`: inherit/fork the complete relevant parent context.

Prefer:

- fresh context for ensembles and independent reviews;
- bounded context for most research/implementation delegation;
- full context when the worker is truly continuing the parent's line of work.

Do not replicate huge histories into every worker if a small task packet is sufficient.

### Communication

Use the cheapest adequate channel:

1. concise return value;
2. structured summary;
3. durable artifact/file;
4. direct agent message;
5. shared task state.

Do not require files for every worker. Files are useful when output is large, reused by several downstream workers, needs persistence, or would pollute the leader context.

### Verification

Choose only what materially improves confidence:

- direct tests/checks;
- independent critic;
- fresh judge;
- consensus;
- no extra verifier.

A separate synthesizer is not mandatory. The leader should synthesize by default unless independence itself is valuable.

### Isolation

Prefer the weakest isolation that prevents interference:

- shared read-only access;
- explicit file/module ownership;
- separate worktrees or sandboxes;
- separate external environments.

Parallel writers to overlapping files are usually a bad partition.

### Stopping

Every nontrivial multi-agent run needs an implicit or explicit stopping rule.

Useful rules:

- requested work complete;
- tests/criteria pass;
- no material new findings;
- no progress across another iteration;
- confidence is sufficient for the decision;
- effort/token/time budget reached.

Do not keep spawning agents merely because more work is theoretically possible.

## 4. Routing Heuristic

Use this decision sequence:

1. **Would independent work materially improve speed, breadth, or confidence?**
   - No → stay single-agent.
   - Yes → continue.

2. **Do later steps strongly depend on earlier outputs?**
   - Yes → use a chain, possibly with parallel waves inside individual stages.

3. **Are the independent facets already obvious?**
   - Yes → fan out directly.

4. **Do independent facets likely exist but need discovery?**
   - Yes → scout → fan-out.

5. **Is the central uncertainty which explanation, design, or answer is correct?**
   - Yes → ensemble.

6. **Can success be evaluated objectively or by a strong focused reviewer?**
   - Yes → add a critic loop or verification node where useful.

7. **Does the task mix dependency barriers and parallelizable stages?**
   - Yes → use a wavefront/DAG.

8. **Are there many dynamically claimable tasks and does the harness support shared coordination?**
   - Yes → work queue/team or native workflow runtime.

9. **Would another agent add distinct information or merely repeat existing work?**
   - Spawn only if the expected information gain exceeds coordination cost.

## 5. Agent Count and Model Choice

Start with the minimum number of agents that gives useful diversity or parallelism.

Prefer expansion-on-evidence:

- one concrete worker when sufficient; use a scout only when discovery itself is substantial independent work;
- inspect results;
- add targeted workers only for unresolved or newly discovered branches.

Do not hard-code `2-4`, `3-5`, or another count as a universal rule.

Do not use the strongest model for every role by default.

Use stronger models where reasoning quality has high leverage:

- topology selection;
- ambiguous decomposition;
- synthesis/judgment;
- difficult debugging;
- final verification of high-risk changes.

Use cheaper/faster models when work is broad but mechanically bounded:

- inventory;
- repetitive file inspection;
- search;
- extraction;
- simple package-local changes;
- deterministic verification.

Honor user-specified model choices and harness limitations.

## 6. Delegation Packets

Every delegated task should contain only the context needed to perform it well.

A strong task packet usually has:

- mission: what the overall run is trying to accomplish;
- assignment: the worker's precise responsibility;
- boundaries: what not to duplicate or modify;
- relevant context: concise facts, paths, artifacts, prior findings;
- expected output: what should be returned or produced;
- verification: checks the worker should run before declaring completion;
- escalation: what uncertainty should be reported rather than guessed.

For parallel workers, tell each worker the other facets or ownership boundaries when that prevents overlap.

Avoid pasting full upstream transcripts by habit.

## 7. Dynamic Adaptation

The topology may change during the run.

Examples:

- fan-out discovers a shared root cause → collapse into one focused chain;
- scout identifies five independent modules → expand into a wave;
- ensemble reaches consensus quickly → skip debate;
- verifier finds one subsystem responsible → spawn only a targeted fixer;
- work queue drains except for one dependency chain → finish sequentially.

Treat the initial topology as a hypothesis, not a contract.

## 8. User Interaction

Do not interrupt the user merely to approve internal delegation.

Ask for input when:

- two plausible paths imply materially different product behavior;
- the requested scope is genuinely ambiguous and cannot be resolved from available context;
- an irreversible or externally consequential action needs their decision;
- the harness requires explicit approval.

Otherwise proceed with the most reasonable topology and keep the user informed at useful milestones.

## 9. Harness Adaptation

Detect the primitives actually available in the current runtime and map this policy onto them.

Read the appropriate reference when needed:

- `references/claude-code.md`
- `references/codex.md`
- `references/patterns.md`
- `references/examples.md`
- `references/research.md`

Do not emulate a richer primitive when the harness already provides it natively.

Examples:

- native shared task graph → use it for a work queue;
- native dynamic workflow/script runtime → use it for high-width deterministic orchestration;
- only parent-managed subagents → use a leader-managed DAG;
- fresh/full/bounded fork controls → choose context intentionally per role.

## 10. Compact Execution Algebra

Conceptually, multi-agent workflows are compositions of:

```text
spawn(task, role?, context?, isolation?, model?)
send(agent, delta)
join(agents, condition=all|enough|first_success)
handoff(from, to, artifact?)
evaluate(output, criteria)
iterate(step, stop_condition)
checkpoint(artifact)
```

The skill does not require these exact tool names. Use the closest primitives exposed by the harness.

The important behavior is the composition, not the API spelling.

## 11. Anti-Patterns

Avoid:

- fan-out because a task merely feels "big";
- mandatory scout before obvious fan-out;
- mandatory user confirmation before delegation;
- mandatory findings files;
- mandatory separate synthesis agent;
- all-to-all agent communication;
- uncontrolled reviewer loops;
- multiple writers on the same files without explicit ownership;
- full-history forks for every worker;
- maximizing agent count instead of information gain;
- forcing a static topology after evidence invalidates it;
- assigning all agents the strongest/most expensive model without reason.

When in doubt, use fewer agents, sharper boundaries, smaller context packets, and stronger verification.

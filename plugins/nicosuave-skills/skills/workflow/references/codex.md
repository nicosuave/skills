# Codex Mapping

Use the orchestration policy in `SKILL.md` and map it onto the multi-agent primitives exposed by the current Codex build.

Modern Codex multi-agent support is richer than a simple fire-and-forget fork model. Where available, use its context-fork controls, persistent task identities, messaging/follow-up, waiting, and agent-tree management intentionally.

## 1. Natural Codex topology

Codex is especially well suited to:

```text
leader-managed chains
fan-out
scout → fan-out
ensembles
critic loops
wavefront / DAG orchestration
```

Even when workers can message or be resumed, the natural model remains a task tree/DAG coordinated by a lead rather than assuming a decentralized shared work queue.

If a future/current build exposes richer shared coordination, use it. Do not fake one if it does not.

## 2. Context Forking

When supported, choose context inheritance deliberately.

Conceptual modes:

```text
fresh / no history
bounded recent history
full history fork
```

### Fresh

Use for:

- ensemble candidates;
- independent review;
- alternative architectures;
- workers that need only a clean task packet.

This reduces anchoring and unnecessary context duplication.

### Bounded

Use for:

- workers that need recent decisions but not the whole thread;
- follow-on implementation after a recent planning turn;
- research branches where a compact amount of recent conversation is useful.

### Full

Use for:

- true continuation of the parent reasoning;
- debugging where the complete investigative path matters;
- a worker inheriting nuanced constraints that are expensive to restate.

Do not default every worker to full history.

## 3. Persistent Tasks and Follow-ups

When workers have persistent task identities, prefer reusing the same worker when follow-up work is tightly coupled to its existing context.

Examples:

```text
worker investigates parser bug
→ lead asks targeted follow-up on one discovered call path
```

```text
module owner implements change
→ verifier finds one package-local defect
→ same module owner receives follow-up
```

Spawn a fresh worker instead when independence is useful:

```text
implementation complete
→ fresh reviewer
```

## 4. Messaging

Direct agent messaging is useful but should remain sparse.

Send a message when:

- one worker discovered a fact that changes another worker's task;
- a leader needs to redirect ongoing work;
- a dependent worker can start early based on a stable partial result;
- a worker should stop pursuing an invalidated branch.

Do not create conversational mesh networks among all workers.

Prefer a task graph with targeted edges.

## 5. Waiting and joins

Use waits as dependency barriers, not as ritual.

Examples:

```text
join(all)
```

when all facets are required.

```text
join(enough)
```

when a decision can be made after enough independent evidence arrives.

```text
join(first_success)
```

when workers are trying alternative reproductions or search strategies and only one success is needed.

If the actual tool only exposes "wait for updates", implement the semantic join at the lead level.

Do not block on a slow worker whose output is no longer needed.

## 6. Agent Tree Management

If the runtime exposes a live agent tree:

- keep task names semantically meaningful;
- inspect live status before spawning duplicate work;
- close workers that are no longer needed;
- reuse a still-relevant worker for tightly coupled follow-ups;
- do not leave completed agents open indefinitely when they consume concurrency slots.

Task names should describe ownership, not personalities:

```text
security_auth
postgres_compat
migration_tests
query_planner
```

rather than:

```text
alice
bob
smart_agent
```

## 7. Model and reasoning routing

When Codex allows per-agent model or reasoning overrides:

Use higher-capability/higher-reasoning settings for:

- ambiguous root-cause investigation;
- architecture;
- synthesis/judgment;
- high-risk review.

Use cheaper/lower-latency workers for:

- repository inventory;
- mechanical extraction;
- repetitive package-local work;
- deterministic check/fix loops where tool feedback dominates.

Do not override inherited settings unless there is a reason.

## 8. Good Codex pattern mappings

### Fan-out

```text
lead
→ spawn security_review    [fresh/bounded]
→ spawn perf_review        [fresh/bounded]
→ spawn correctness_review [fresh/bounded]
→ wait for useful results
→ integrate
```

### Scout → Fan-out

```text
lead
→ spawn repository_scout
→ scout returns dependency map + suggested facets
→ lead dynamically spawns only valuable facets
→ follow-up targeted workers as evidence warrants
```

No user checkpoint is required merely because the graph expands.

### Ensemble

```text
lead
→ spawn hypothesis_race      [fresh]
→ spawn hypothesis_state     [fresh]
→ spawn hypothesis_ordering  [fresh]
→ judge/reconcile
→ focused reproduction
```

### Critic loop

```text
implementer
→ fresh reviewer or test diagnostics
→ follow-up implementer
→ verify
```

Use the same implementer for revisions when its local context is useful, but a fresh reviewer for independence.

### Wavefront

```text
[api_inventory, storage_inventory, test_inventory]
→ planner
→ [api_changes, storage_changes]
→ integration
→ verifier
```

Start each downstream task as soon as its actual dependencies are satisfied.

## 9. Files and durable artifacts

Do not require every worker to write to a global findings directory.

Use files/artifacts when:

- output is too large for a concise return;
- several downstream workers need the same result;
- persistence across compaction/resume matters;
- a structured artifact is itself useful to the user;
- the worker is generating code/docs rather than merely reporting.

Otherwise a structured worker result is enough.

If using artifacts, prefer session/project-local locations over a globally shared `~/.agents/findings` convention unless there is a specific reason to persist there.

## 10. When Codex differs from richer team runtimes

Do not assume:

- shared self-claiming task queues;
- all teammates see a common task board;
- decentralized peer coordination;
- nested teams.

If those primitives are unavailable, express the workflow as a lead-managed DAG.

That is not a limitation of the orchestration policy: it is simply a different implementation of the same topology.

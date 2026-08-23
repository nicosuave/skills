# Research Basis

The orchestration policy is intentionally based on recurring findings from recent agent-system research rather than one framework's API.

The durable conclusions are more important than any single benchmark number.

## 1. Workflows are composable patterns

Anthropic's "Building Effective Agents" separates several useful patterns:

- prompt chaining;
- routing;
- parallelization;
- orchestrator-workers;
- evaluator-optimizer.

The important lesson for this skill is that these are building blocks, not a single canonical workflow.

Reference:

- Anthropic, *Building Effective Agents*  
  https://www.anthropic.com/engineering/building-effective-agents

## 2. Better topology beats indiscriminate scaling

Recent multi-agent optimization work increasingly treats agent topology itself as something to optimize.

### MASS — Multi-Agent System Search

MASS studies joint optimization of agent prompts and topologies and argues against the assumption that simply adding more agents is an efficient path to better performance.

Reference:

- Google Research, *Multi-Agent Design: Optimizing Agents with Better Prompts and Topologies*  
  https://research.google/pubs/multi-agent-design-optimizing-agents-with-better-prompts-and-topologies/

Implication for this skill:

> Spawn only when a worker is likely to add distinct information or useful parallelism.

## 3. Communication graphs should react to runtime evidence

Dynamic/self-organizing multi-agent research explores communication structures that adapt based on state or agent utility rather than using a fixed all-to-all or hierarchical graph.

Relevant themes include:

- state-conditioned communication graphs;
- dynamically constructed DAGs;
- selective agent participation;
- pruning redundant edges.

Implication:

> The initial topology is a hypothesis, not a contract.

## 4. Sparse communication can be better

AgentPrune and related work show that redundant agent communication can be removed aggressively without necessarily harming performance, while substantially reducing token usage.

Reference:

- *Cut the Crap: An Economical Communication Pipeline for LLM-based Multi-Agent Systems* / AgentPrune  
  https://research.cuhk.edu.hk/en/publications/cut-the-crap-an-economical-communication-pipeline-for-llm-based-m/

Implication:

- do not build all-to-all debate meshes by default;
- send only useful deltas;
- use a judge instead of endless mutual critique;
- keep shared context small.

## 5. Workflow search / graph optimization

AFlow treats agent workflows as graphs/programs and searches over them rather than hand-selecting one fixed architecture.

Reference:

- *AFlow: Automating Agentic Workflow Generation* (ICLR 2025)  
  https://proceedings.iclr.cc/paper_files/paper/2025/hash/5492ecbce4439401798dcd2c90be94cd-Abstract-Conference.html

Implication:

> Think of orchestration as composition over a small execution algebra.

This is why `SKILL.md` separates:

```text
topology
context
communication
verification
isolation
stopping
```

instead of baking them into one phase protocol.

## 6. Production research systems favor lead + specialized workers

Anthropic's production multi-agent research architecture uses a lead agent that plans/decomposes dynamically and delegates specialized research threads, updating direction based on intermediate evidence.

Reference:

- Anthropic, *How we built our multi-agent research system*  
  https://www.anthropic.com/engineering/multi-agent-research-system

Implication:

- scout → fan-out remains an excellent topology for broad research;
- the error in the old multiclaude skill was treating it as universal;
- decomposition should be adaptive;
- intermediate agent outputs should be compressed before reaching the lead.

## 7. Ensembles are valuable when independence is preserved

Independent hypotheses or solutions are useful because agents can explore different local optima. If candidates see one another's reasoning too early, anchoring reduces the diversity you were paying for.

Implication:

Use fresh contexts for first-pass ensemble candidates, then reconcile.

## 8. Deterministic evaluation should dominate conversational review when possible

When a compiler, test, benchmark, linter, static analyzer, or other objective check exists, it should usually be the critic-loop signal rather than another general-purpose "reviewer" conversation.

Implication:

```text
change → deterministic check → targeted repair
```

is often better than:

```text
change → generic reviewer → generic reviewer → ...
```

## 9. Harness capabilities change the optimal implementation

Conceptual topology should survive harness evolution.

Examples:

- a parent/worker runtime naturally implements leader-managed DAGs;
- a shared task board naturally implements work queues;
- a workflow-script runtime naturally implements high-width deterministic graphs;
- context-fork controls let an ensemble remain independent without manual transcript surgery.

This is why harness-specific mechanics live in separate reference files.

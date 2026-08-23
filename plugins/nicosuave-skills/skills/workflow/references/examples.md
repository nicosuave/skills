# Examples

These are pattern examples, not mandatory recipes.

## Deep repository architecture review

User intent:

```text
Go deep on whether this repository is organized well and where the package boundaries are wrong.
```

Routing:

```text
known broad quality dimensions exist
but exact subsystem decomposition is unclear
→ scout + fan-out + wavefront
```

Possible execution:

```text
repo scout
  → identifies packages, build graph, duplicated abstractions, hot coupling

parallel wave:
  - package boundary analysis
  - build graph / incremental build analysis
  - shared infrastructure / duplication analysis
  - runtime dependency / layering analysis

leader integrates
  → targeted follow-up on 1-2 disputed boundaries
  → final recommendations
```

No user confirmation between scout and fan-out unless the scout exposes a product-level ambiguity.

---

## Flaky integration test

Routing:

```text
uncertain root cause
→ ensemble hypotheses
→ focused chain
→ critic/verification loop
```

Execution:

```text
fresh hypothesis A: race/concurrency
fresh hypothesis B: environment/state leak
fresh hypothesis C: ordering/time dependence

judge picks strongest evidence
→ reproduce mechanism
→ patch
→ repeated test / stress verification
→ fresh reviewer if risk warrants
```

---

## Large migration across hundreds of files

Routing:

```text
many similarly shaped independent items
+ deterministic checks
→ native workflow/work queue when available
```

Execution:

```text
inventory migration shapes
→ generate task shards
→ workers migrate disjoint shards
→ run format/type/test checks
→ failed shards requeued
→ global integration verifier
```

Do not give 500 files to 500 expensive reasoning agents without grouping when a deterministic transformation or smaller number of shards will do.

---

## Open-ended technical research

Routing:

```text
parallelizable uncertainty exists
decomposition not yet obvious
→ scout → fan-out
```

Execution:

```text
scout:
  - map major approaches
  - identify primary sources
  - identify contested claims
  - suggest 3 useful facets

fan-out:
  - approach A
  - approach B
  - benchmark/evidence quality
  - implementation constraints

leader integrates
→ optionally spawn one targeted contradiction resolver
```

Do not require a separate synthesizer unless a fresh judge is useful.

---

## Security review

Two common variants.

### Orthogonal surface review

```text
fan-out:
  auth/session
  injection/data validation
  secrets/crypto
  authorization/business logic
  dependency/supply-chain
→ integrate
```

### Adversarial independent review

```text
ensemble:
  several fresh reviewers inspect same high-risk flow
→ judge merges only evidenced findings
```

Use the first for broad coverage, the second for high-risk ambiguous flows.

---

## Feature spanning several packages

If boundaries are known:

```text
planner
→ parallel package owners
→ integration owner
→ tests
```

If boundaries are unknown:

```text
scout/planner
→ identify ownership and interfaces
→ parallel owners
→ integration
```

If workers would all edit the same central types, do not parallelize implementation blindly. Keep the central refactor sequential, then fan out leaf migrations.

---

## Performance regression

Routing:

```text
multiple plausible bottlenecks
→ small ensemble or fan-out by subsystem
→ measure
→ focused optimization chain
```

Execution:

```text
parallel:
  CPU/profile analysis
  allocation/memory analysis
  IO/query analysis

leader compares against measurement
→ select bottleneck
→ optimize
→ benchmark
→ stop when target reached/no meaningful progress
```

Measurement is the critic.

---

## Compatibility matrix / feature coverage research

Routing:

```text
independent feature domains are obvious
→ direct fan-out
```

Execution:

```text
protocol/query syntax
types/functions
DDL/DML
transactions
metadata/catalog
drivers/tooling

workers return structured rows
→ leader builds one comparison matrix
```

A scout would be unnecessary unless the compared systems are unfamiliar enough that the categories themselves are uncertain.

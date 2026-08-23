---
name: context-engineering
description: "Design, audit, and refactor agent context: AGENTS.md or CLAUDE.md files, skills, scoped reference docs, subagent task packets, handoffs, and durable working artifacts. Use when context is bloated, duplicated, stale, poorly routed, missing completion criteria, or when deciding what belongs inline versus behind a conditional pointer."
---

# Context Engineering

Use this skill when **context itself is the object of work**. Do not invoke it merely because every task has context.

The governing principle is:

> Put each fact, instruction, and artifact in the narrowest context that reliably reaches every consumer that needs it, and no others. Keep sources canonical, pointers conditional, packets bounded, and completion criteria observable.

The goal is not minimum tokens at any cost. The goal is **high signal at the moment of use** with low duplication and low variance.

## 1. Classify What You Are Writing

Before editing context, identify which kind of object it is.

- **Source of truth**: code, config, tests, schemas, specifications, issue bodies, or another canonical artifact that owns a fact.
- **Instruction**: durable behavior the agent should follow.
- **Pointer**: a compact condition that tells the agent when and why to load another source.
- **Task packet**: bounded context for one delegated assignment or execution step.
- **Working state**: current facts, decisions, unknowns, blockers, and next actions for an active effort.

Do not turn one object into another without reason. In particular:

- do not copy machine-readable facts from code or config into prose;
- do not promote one task's transient state into global instructions;
- do not bury reusable behavior inside a one-off task packet;
- do not use a pointer so vague that the target is never loaded.

## 2. Choose the Narrowest Reliable Placement

Use the information hierarchy below. Move information upward only when narrower placement would make it unreliable.

| Placement | Put here | Keep out |
|---|---|---|
| User-level `AGENTS.md` | Durable preferences and rules that apply across nearly all projects | Project facts, undated version-specific behavior, one-off workflows |
| Project-root `AGENTS.md` / `CLAUDE.md` | Project-wide invariants, terminology, commands that are not cheaply discoverable | Subsystem details, copied configuration, temporary plans |
| Scoped instruction file | Rules and context for one package, service, directory, or domain | Unrelated project-wide guidance |
| Skill description | The conditions that should trigger the skill and the capability it provides | The full process or reference material |
| Skill body | Reusable process, decision rules, completion criteria, and guardrails | Large branch-specific references that only some runs need |
| Referenced document | Detailed material needed only on a particular branch | Rules every run needs before it can begin |
| Task packet / handoff | One assignment's mission, boundaries, evidence, expected output, and verification | Full conversation history by default |
| Code, config, tests, specs | Facts the environment can authoritatively expose | Prose copies that will become stale |

Prefer scoped files over expanding a root instruction file. Prefer a pointer over duplicating a long body. Prefer inspecting the environment over caching trivial lookups in prose.

## 3. Write Conditional Pointers

A useful pointer contains three things:

```text
trigger + target + purpose
```

Weak:

```markdown
See `docs/security.md`.
```

Strong:

```markdown
Before changing authentication, session, or token handling, read `docs/security.md` for the threat model and required invariants.
```

Pointer rules:

- Lead with the condition that should fire it.
- Name the target exactly.
- Say what the target contributes.
- Use one pointer per genuinely distinct branch; do not pad it with synonyms.
- Keep universally required behavior inline rather than hiding it behind a pointer.
- Do not restate the target's contents beside the pointer.

A pointer is successful when relevant runs reliably load the target and unrelated runs do not.

## 4. Use Progressive Disclosure

Inline what every branch needs. Put branch-specific detail behind a pointer and load it only when that branch becomes active.

Split material when one of these is true:

- only a subset of runs needs it;
- it is volatile and should be maintained independently;
- it is large enough to obscure the controlling process;
- another artifact already owns it;
- the next phase should receive a bounded artifact rather than the entire prior transcript.

Do not split merely to create more files. Every split must improve routing, ownership, or legibility enough to pay for another pointer.

## 5. Build Bounded Task Packets

When handing work to another agent or session, provide the smallest context that permits correct independent work.

A strong packet usually contains:

- **Mission**: what the overall effort is trying to achieve.
- **Assignment**: the worker's precise responsibility.
- **Boundaries**: what it must not duplicate, modify, or decide.
- **Relevant context**: concise facts plus exact paths, issues, commits, or artifacts to inspect.
- **Expected output**: the result or artifact to return.
- **Verification**: checks that must pass before completion.
- **Escalation**: uncertainties that should be reported rather than guessed.

Use pointers to canonical material instead of pasting it. Include full history only when the worker truly continues the same line of reasoning and compression would remove load-bearing context.

## 6. Make Completion Observable

Every nontrivial instruction or workflow step needs a completion criterion that lets the agent distinguish done from merely progressed.

Prefer criteria such as:

- every modified interface has a corresponding behavior test;
- the clean case passes, the deliberate violation fails, and the restored case passes again;
- every material claim is tied to a primary source;
- every branch in the enumerated case set is implemented and tested;
- the generated artifact exists at the requested path and validates against its schema.

Avoid criteria such as "be thorough," "understand the code," or "improve quality." They create motion without a stopping boundary.

Use the strongest cheap observable signal available: tests, compiler output, schema validation, benchmark thresholds, explicit acceptance criteria, or a focused independent review.

## 7. Prune Aggressively, Not Mechanically

Audit each line for the behavior it changes.

Remove or relocate:

- **duplication**: the same meaning has more than one source of truth;
- **stale caches**: prose copies of facts cheaply discoverable from the environment;
- **no-ops**: instructions that do not materially change model behavior;
- **sediment**: once-useful guidance whose triggering condition no longer exists;
- **mis-scoped rules**: project or subsystem detail loaded globally;
- **aspirations**: vague preferences without an observable target;
- **version traps**: exact tool or model behavior stated without a date, version, or verification condition;
- **conflicts**: overlapping instructions whose precedence is unclear.

Prefer positive target behavior over mentioning the failure mode repeatedly. Keep explicit prohibitions when they protect user work, security, irreversible actions, or another hard boundary; pair them with the desired behavior.

Do not optimize for shortness by deleting necessary caveats, provenance, or safety constraints.

## 8. Lightweight Decision State

For nontrivial work, maintain a compact internal state model:

```text
facts | unresolved questions | user-owned decisions | blockers | frontier
```

Update it as evidence arrives. The **frontier** is the work or decisions whose prerequisites are currently satisfied.

This is working state, not mandatory ceremony:

- keep it implicit for small tasks;
- persist it only when the effort spans sessions, agents, or durable artifacts;
- do not turn every unknown into an issue;
- do not ask the user about facts the agent can inspect;
- ask only when user judgment materially changes product behavior, scope, risk, or an irreversible action.

## 9. Refactoring Process

When auditing or rewriting context:

1. **Inspect before editing.** Read the current instruction files, referenced docs, relevant skills, and the environment they describe.
2. **Map consumers and triggers.** For each material rule, identify who needs it, on which branch, for how long, and what failure it prevents.
3. **Find the owner.** Decide whether the content belongs in code/config/tests/specs, an instruction, a pointer, a skill, a scoped doc, or a task packet.
4. **Refactor minimally.** Preserve user-authored intent and established terminology. Prefer small relocations and sharper pointers over wholesale rewrites.
5. **Add observable bounds.** Strengthen vague processes with completion criteria and stopping rules.
6. **Validate routing.** Check that relevant tasks discover the material, unrelated tasks avoid it, and no canonical fact is duplicated.
7. **Report the semantic delta.** Summarize which behavior changed, what moved, and what was deliberately left alone.

Do not require a separate report file unless the user requests one or the audit is large enough to need a durable artifact.

## 10. Work With The Workflow Skill

`workflow` chooses the orchestration topology. `context-engineering` chooses what each node receives, what remains canonical, and how information crosses phase boundaries.

When a workflow delegates:

- use bounded task packets;
- send deltas instead of entire histories;
- keep independent ensemble contexts independent;
- persist artifacts only when multiple downstream nodes need them or the result must survive the run;
- use handoffs at real context boundaries, not as ritual between every step.

Topology and context propagation are separate decisions. Choose both intentionally.

## 11. Anti-Patterns

Avoid:

- expanding a root `AGENTS.md` for every newly discovered project detail;
- copying commands or settings that are obvious from `package.json`, CI, or config;
- pointers that say only "see also";
- task packets containing the full transcript by default;
- one giant skill body containing every branch and harness detail;
- mandatory context summaries at every phase boundary;
- completion criteria expressed only as prose quality;
- rewriting user instruction files during unrelated work;
- optimizing token count while making invocation less reliable;
- preserving stale guidance because deletion feels riskier than verification.

When in doubt, keep the canonical source, sharpen the pointer, narrow the scope, and add an observable done condition.

Read `references/examples.md` when concrete pointer, placement, packet, or audit examples would help.

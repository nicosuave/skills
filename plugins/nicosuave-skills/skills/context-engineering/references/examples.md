# Context Engineering Examples

Load this reference only when concrete examples are useful.

## 1. Conditional Pointers

### Weak

```markdown
See `docs/data-model.md`.
```

The agent cannot tell when to load it or what it contributes.

### Strong

```markdown
Before changing event schemas, projections, or replay behavior, read `docs/data-model.md` for event invariants and compatibility rules.
```

The trigger, target, and purpose are explicit.

### Too broad

```markdown
For backend work, read `docs/backend.md`.
```

Nearly every task triggers it. Either the content is universally needed and should be inline, or the pointer needs narrower branches.

### Better split

```markdown
Before changing authentication or token validation, read `docs/backend/auth.md` for threat-model invariants.

Before changing background retries or idempotency, read `docs/backend/jobs.md` for delivery semantics.
```

## 2. Source Of Truth Versus Cached Prose

### Cached and likely to drift

```markdown
Run `bun test`, `bun run typecheck`, and `bun run lint` before finishing.
```

If the scripts are already discoverable from `package.json`, a more durable instruction is:

```markdown
Run the repository's relevant test, type-check, lint, and build commands before finishing; inspect the current scripts rather than assuming their names.
```

Keep exact commands in prose only when they encode a non-obvious convention or the lookup is genuinely expensive.

## 3. Root Versus Scoped Instructions

### Bloated root file

```markdown
When modifying the iOS notebook renderer, preserve cell identity during diff application, rebuild the preview host, and run RendererSnapshotTests.
```

### Scoped placement

Root `AGENTS.md`:

```markdown
Before changing a subsystem, read the nearest scoped `AGENTS.md` or `CLAUDE.md`.
```

`ios/notebook/AGENTS.md`:

```markdown
Preserve cell identity during renderer diff application. After renderer changes, rebuild the preview host and run `RendererSnapshotTests`.
```

## 4. Bounded Delegation Packet

```markdown
## Mission

Determine whether DuckFlight can validate OIDC access tokens without coupling the core protocol layer to one identity provider.

## Assignment

Inspect the current authentication implementation and identify the smallest stable verification interface.

## Boundaries

Do not edit code. Do not evaluate UI login flows. Do not repeat the separate JWKS caching investigation.

## Relevant context

- `crates/server/src/auth/`
- issue #184
- `docs/auth-threat-model.md`

## Expected output

A concise recommendation containing the proposed interface, supported token claims, failure modes, and affected modules.

## Verification

Every statement about current behavior must cite the exact source path or test. Distinguish observed behavior from the proposed design.

## Escalation

Report any product decision about accepted issuers or tenant isolation rather than guessing.
```

## 5. Completion Criteria

### Weak

```markdown
Review the migration carefully.
```

### Strong

```markdown
The migration is complete when every old-column read has moved to the new representation, both representations remain writable during the compatibility window, the old path has no remaining callers, and the relevant migration and integration tests pass.
```

## 6. Lightweight Decision State

For a long-running effort, a small durable artifact can look like:

```markdown
## Facts

- The protocol currently authenticates only at connection setup.
- Clients may reconnect to a different worker.

## Decisions

- Token verification remains stateless at the worker.

## Unknowns

- Whether tenant membership is embedded in the token or loaded separately.

## Blockers

- Issuer and audience policy must be defined before the claim contract is final.

## Frontier

- Compare claim contracts used by three relevant database servers.
- Prototype the verifier interface against two token shapes.
```

Do not persist this for a small task. The artifact pays for itself only when several sessions or agents need the state.

## 7. Audit Questions

Use only the questions relevant to the artifact being reviewed:

- Who consumes this line?
- What exact condition makes it relevant?
- What failure does it prevent?
- Is another artifact the canonical owner?
- Could the environment expose the fact directly?
- Is the rule durable at this scope?
- Does the pointer say when and why to load its target?
- Can the agent observe when the instruction is complete?
- Would deleting the line materially change behavior?
- Does it conflict with a narrower or newer instruction?

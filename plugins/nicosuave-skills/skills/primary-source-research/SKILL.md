---
name: primary-source-research
description: "Use for explicit primary-source research or substantive external questions requiring comparison of evidence, resolution of conflicting claims, or provenance tracing. Not for quick factual lookups, routine code inspection, supplied-material summaries, or prior-session recall."
---

# Primary Source Research

Use sources that can directly establish the claim. Secondary sources are useful for discovery, terminology, and competing interpretations; they should not carry a material factual claim when an accessible primary source owns it.

Match depth to the requested answer. These sections are decision aids, not a checklist to execute on every question. A focused authoritative lookup can be sufficient; stop once the material uncertainty is resolved. Being technical, current, or precise does not by itself require a research workflow.

For prior-session questions, retrieve the relevant history (prefer Memex when available). For supplied screenshots or text, explain that material first. Verify current external behavior only when the user asks or the answer depends on it; do not silently turn recall or interpretation into a fresh investigation. Do not clone repositories or trace implementation merely because code is a possible source; use that depth when the question requires it or cheaper evidence leaves a consequential gap.

The governing principle is:

> Trace each material claim to the closest source with authority to establish it. Preserve version, date, scope, and provenance. Separate what the source says from what you infer.

## 1. Frame The Research Precisely

Before searching, identify:

- the question to answer;
- the decision or deliverable the answer will support;
- the relevant time, version, branch, jurisdiction, population, or product tier;
- the material claims that must be established;
- what level of uncertainty is acceptable.

Do not expand a narrow question into a general survey unless the surrounding decision requires it.

For complex work, maintain a lightweight claim ledger:

```text
claim | source owner | evidence found | scope/version | status
```

This can remain internal. Persist it only when the research spans agents, sessions, or a durable report.

## 2. Identify The Source Owner

Ask: **what artifact or institution directly owns this fact?**

Examples:

- current software behavior → source code, tests, release artifacts, observed API behavior;
- intended software contract → specification, official API docs, accepted design record;
- shipped change → release notes plus source/tag or deployed behavior, not merely an open issue;
- protocol semantics → normative specification and conformance tests;
- research result → original paper, supplement, dataset, registry, and authors' released code;
- company metrics or obligations → filing, audited report, official dataset, contract, or first-party statement;
- law or policy → statutory text, regulation, court opinion, agency rule or guidance;
- current product limits or prices → official product docs, pricing page, API response, or account interface.

Read `references/source-owners.md` when a domain-specific source hierarchy would help.

Use secondary sources to locate primary material, understand vocabulary, discover disagreement, or identify what to verify. Follow important secondary claims back to their origin.

## 3. Search By Independent Evidence Facet

Search around the claim, not only the user's wording. Include exact identifiers, prior names, version numbers, specification sections, code symbols, authors, and organizations that may own the evidence.

Keep research with one agent unless a concrete, bounded independent task will save enough time or improve confidence to outweigh briefing, duplicated context, and review. Multiple facets or source types alone do not justify delegation. Use the `workflow` skill only if an actual orchestration choice needs it. Potential partitions, after that decision, include:

- separate source-owning organizations;
- implementation versus specification;
- historical behavior versus current behavior;
- independent hypotheses;
- distinct jurisdictions or product tiers.

Do not fan out several agents to run near-identical searches. Preserve independence only when it yields distinct evidence or de-anchored interpretations.

## 4. Inspect The Primary Material, Not Its Snippet

Search snippets, generated summaries, repository topic text, and quoted excerpts are navigation aids, not evidence.

Inspect enough of the underlying source to establish:

- what the source actually says or does;
- whether the relevant language is normative, descriptive, proposed, deprecated, or historical;
- the applicable version, branch, date, product tier, jurisdiction, or population;
- nearby exceptions and definitions;
- whether later material supersedes it.

When implementation behavior is the question, follow the relevant executed path and tests far enough to resolve it. For documents, read the controlling section and any definitions that affect the answer. For datasets, inspect the methodology and coverage relevant to the claim. Expand only when the evidence exposes a material gap.

## 5. Record Evidence With Scope

For each material claim, capture:

- the source;
- the exact section, path, symbol, table, or record supporting it;
- publication or modification date when relevant;
- applicable version or scope;
- whether the evidence is direct or inferential;
- any contradiction or caveat.

Preserve stable links, commit hashes, document identifiers, or citations in durable outputs. Do not cite a homepage when a specific source location is available.

Quote only when the exact wording matters. Otherwise paraphrase accurately and keep the citation attached to the claim.

## 6. Reconcile Conflicts Instead Of Flattening Them

When reliable sources disagree:

1. Check whether they describe different versions, dates, tiers, jurisdictions, environments, or definitions.
2. Prefer the source with direct authority over the claim, but do not silently discard contrary evidence.
3. Distinguish intended contract from observed implementation when they diverge.
4. State the conflict and the basis for any conclusion.
5. Keep uncertainty explicit when the evidence does not resolve it.

Do not infer that a feature shipped because it was proposed, merged, documented, or announced separately. Verify the artifact that establishes the requested state.

Absence of evidence is not evidence of absence unless the source is demonstrably complete for the relevant scope.

## 7. Separate Fact, Inference, And Recommendation

Structure the synthesis so the reader can tell which layer they are seeing.

- **Fact**: directly supported by cited evidence.
- **Inference**: a conclusion drawn from one or more facts; name it as an inference and cite its premises.
- **Recommendation**: a judgment based on evidence plus the user's goals or constraints.
- **Unknown**: unresolved because evidence is missing, inaccessible, ambiguous, or conflicting.

Do not let a plausible narrative outrun the sources.

## 8. Write The Answer, Not A Bibliography Dump

Lead with the answer or decision-relevant conclusion. Support every load-bearing claim close to where it appears.

A strong research result usually contains:

- the answer;
- the evidence that controls it;
- relevant version/date/scope qualifiers;
- important contradictions or uncertainty;
- the practical implication for the user's decision.

Do not produce a separate report file by default. Write a durable artifact when the user asks, when several downstream agents need it, or when the research is too large to preserve accurately in the current context.

## 9. Completion Criteria

Research is complete when:

- every material claim is supported by a primary source when one is accessible;
- secondary claims that matter have been traced to their origin;
- version, date, branch, jurisdiction, tier, or population applicability has been checked;
- source conflicts are explained rather than hidden;
- facts, inferences, recommendations, and unknowns are distinguishable;
- citations or stable pointers are preserved in the final output;
- remaining uncertainty is explicit and material gaps are named.

Stop when the decision is sufficiently supported. Do not continue collecting sources merely because more exist.

## 10. Anti-Patterns

Avoid:

- citation laundering through a secondary article when the original is available;
- treating search snippets or AI summaries as evidence;
- citing an issue, proposal, PR, or announcement as proof of shipped behavior;
- assuming official docs describe the currently executed code path;
- citing source code without identifying the relevant version or branch;
- quoting one sentence without its definitions or exceptions;
- confusing publication date with the date an event occurred;
- using many citations that all derive from the same original source as independent confirmation;
- hiding disagreement among reliable sources;
- producing a large source list without synthesizing the answer;
- asking the user for facts that can be obtained from available primary material.

When in doubt, find the artifact that owns the claim, inspect it in context, and make the scope visible.

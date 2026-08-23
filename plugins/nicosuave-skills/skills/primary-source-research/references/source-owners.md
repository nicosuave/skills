# Source Owners By Domain

Use this reference to choose the source closest to the fact being established. The ordering is contextual rather than absolute: a source is primary only for claims it directly owns.

## Software And APIs

| Claim | Strongest direct evidence |
|---|---|
| Current executed behavior | Source code at the relevant version, tests, reproducible runtime/API behavior |
| Public contract | Normative spec, official API reference, accepted schema or interface definition |
| Why a change was made | Issue, design document, ADR, PR discussion, commit message from the authors |
| Whether a change shipped | Tagged source/release artifact, release notes, package registry, deployed behavior |
| Current limits or compatibility | Versioned official docs plus implementation or conformance evidence where possible |
| Security property | Threat model, protocol spec, source path, tests/audit evidence; not marketing copy |

An issue or PR is primary for intent and discussion, but not automatically for current shipped behavior.

## Standards And Protocols

Prefer:

1. normative specification or RFC at the applicable version;
2. official errata and extension documents;
3. conformance tests and reference implementations;
4. standards-body minutes or accepted proposals for rationale;
5. implementation docs and secondary explanation for interpretation.

Separate normative requirements (`MUST`, `SHOULD`, etc.) from implementation convention.

## Scientific And Technical Research

Prefer:

1. original paper or preprint version actually being discussed;
2. supplementary methods and appendices;
3. registered protocol or trial registry;
4. released dataset and data dictionary;
5. authors' code and experiment configuration;
6. correction, retraction, or later version;
7. systematic review or replication for context and external validity.

A review is primary for its own synthesis, not for the underlying experiments it summarizes.

## Companies, Products, And Markets

Prefer:

- regulatory filings and audited reports for reported financial facts;
- official pricing, product docs, status pages, changelogs, and account/API surfaces for current product behavior;
- contracts, terms, and policy documents for obligations;
- first-party datasets or dashboards for operational metrics;
- direct statements from named company representatives for intent or announcement claims.

Marketing pages may be first-party but are weak evidence for technical or comparative claims.

## Law, Regulation, And Policy

Prefer:

1. statutory or constitutional text;
2. codified regulations and final rules;
3. controlling court opinions and official dockets;
4. agency orders, notices, and formal guidance;
5. legislative or regulatory history for interpretation;
6. official forms and administrative manuals for procedure.

Always identify jurisdiction, effective date, amendment status, and whether a source is binding or advisory.

## News And Current Events

Primary evidence may include:

- official records, filings, transcripts, data releases, or court documents;
- direct statements, recordings, or posts from involved parties;
- contemporaneous images, video, or datasets with verifiable provenance;
- official incident, election, weather, or market data.

Reputable journalism remains valuable for discovery, chronology, corroboration, and facts no public primary record exposes. Do not pretend first-party statements are neutral; authority over the statement does not imply truth about every underlying event.

## Data And Metrics

Inspect:

- dataset owner and collection process;
- field definitions and units;
- population and coverage window;
- revision and backfill policy;
- missingness and exclusions;
- whether values are raw, modeled, seasonally adjusted, sampled, or aggregated;
- release date versus observation date.

A chart or article derived from a dataset is secondary to the dataset for numeric claims, but may be primary for the author's interpretation.

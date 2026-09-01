# Ninety-Day Implementation Sequence and MVP

Status: Recommended sequence following the August 26 discovery session.
Companion documents: `architecture-decision.md`, `system-design.md`, `prd.md`.

Note on scope of this document. This describes what to build, in what order, and how to tell whether each piece worked. It carries no pricing. The sequence and the acceptance tests are what make a plan checkable; the commercial terms are a separate conversation.

---

## The MVP

The first shippable version is smaller than most people would propose, and deliberately so.

> **The MVP is the weekly sweep producing a ranked, reasoned proposal queue, reading from an organizational model that is regenerated on every run.**

No core yet. No extractions yet. No changes to any application.

Why this and not a first extracted capability:

- **It is the instrument.** Extracting before you can measure duplication means guessing which three of sixteen capabilities matter, and the existing survey is not precise enough to tell you. The sponsor's own estimate was "five or six or twenty."
- **It is read-only,** so it cannot break anything, which means it ships while eight people keep building.
- **It produces value on day one of its existence** even if nothing else follows, because the queue is a standing artifact anyone can act on.
- **It works at Station,** where the honest answer is zero, and zero is a correct and useful answer for an organization that has not started yet.
- **It is the cheapest thing to be wrong about.** A read-only agent over five repositories is two weeks of work with almost no downside risk.

Everything after the MVP is fed by it.

---

## Sequence

Five stages, sequential with deliberate overlap, across roughly thirteen weeks. Each has a single acceptance test. If the test does not pass, the stage is not finished.

---

### Stage 1 - Ground truth: the estate and the tool layer

**Weeks 1 to 3.**

The largest unknown in this project is how the fifty-one packaged items reach the systems they touch. Every downstream estimate depends on the answer, so it gets bought first.

Work:

- Read-only pass across the five repositories. Verify the fifty-one items, the thirty-one applications, and the sixteen capabilities against what is actually in source.
- Map the tool layer: for every application, how it reaches every system of record. MCP server, direct API, custom integration, or human in the loop.
- Identify which items are live and which are dormant. A duplication count that includes dead code overstates the problem and misdirects the first extractions.
- Record credential and access risk observed along the way, packaged as a handoff for whoever takes the IT and security work. Observation only, no remediation.
- Station discovery: interviews with Station Austin leadership, then Northwest Arkansas and DC as availability allows. Find repeated, human-intensive work. Event programming and grant writing are the two named candidates.

Outputs: verified estate register with live-versus-dormant status, tool-layer map, access and credential risk handoff, Station opportunity notes.

**Acceptance test:** for any application named at random, the map says what it touches and how, and the answer checks out in source.

---

### Stage 2 - The organizational model

**Weeks 3 to 5.**

A machine-readable description of every capability in the estate, generated from the repositories rather than hand-maintained, in a format that ports if a first-party registry ships.

Work:

- Model schema and generator across the five entity types: application, capability, tool, deployment, data source.
- Model-readable descriptions for every capability, treated as an API surface rather than documentation.
- Ownership, consumers, systems touched, and evaluation status per capability.
- First full generation across all five repositories.
- Description quality report, since a weak description is a false negative waiting to happen.

Outputs: model generator, populated model, description quality report.

**Acceptance test:** a model, given only the organizational model, correctly answers "do we already do this?" for ten capabilities chosen by the sponsor, including at least three where the answer should be no.

---

### Stage 3 - The feedback loop

**Weeks 5 to 8. This completes the MVP.**

The weekly sweep, which is both the extraction mechanism and the drift auditor.

Work:

- Weekly scheduled read-only pass over the estate.
- Duplication and divergence detection.
- The four-criterion extraction test applied to every finding, producing one recommendation per item: extract, patch in place, or leave alone.
- Ranked queue with reasoning attached to each item, criterion by criterion.
- Operator console, first version: the queue, the model, coverage gaps, and the disposition of prior weeks' proposals.

Outputs: running weekly sweep, first proposal queue with reasoning, operator console.

**Acceptance test:** the sweep independently rediscovers the duplication the original survey found, and either explains any item it disagrees with or is corrected. Disagreement that survives scrutiny is a finding, not a failure.

---

### Stage 4 - The MCP layer

**Weeks 7 to 10.**

Making the understanding available to Claude, which is the only part of the system that prevents duplication rather than recovering from it.

Work:

- MCP server exposing `capability_search`, `capability_get`, `application_list`, `standards_get`, `systems_describe`, and `usage_log`.
- Semantic matching on described behaviour rather than names.
- Distribution to the eight builders' Claude environments.
- PRD decomposition into candidate capabilities, and wiring to matches during planning.
- Query telemetry feeding the sweep, with non-matches ranked by how many separate PRDs asked for the same missing thing.
- Verification that the invisibility constraint holds: the builder writes a PRD, says go, and never learns the layer exists.

Outputs: MCP server in the builders' environments, plan-time reuse working end to end, query telemetry.

**Acceptance test:** a new application is planned start to finish. The search surfaces at least one existing capability that would otherwise have been rebuilt, and the builder is shown nothing.

---

### Stage 5 - The first extractions and handover

**Weeks 10 to 13.**

The core, filled from evidence rather than from the survey.

Work:

- Extract the top three approved proposals. Not a target of three, a cap of three: extraction rate is limited by evaluation coverage, and the estate currently has one evaluation suite across fifty-one items.
- Each extracted capability gets a contract, an owner, a version, an evaluation, and a known consumer list.
- Consumer applications updated one at a time, with rollback demonstrated on at least one.
- Per-function model routing demonstrated on whichever extracted capability has more than one analysis function, with a measured cost delta.
- Handover: runbooks, the extraction test as a written standard, ownership recommendation, and the next tranche.

Outputs: three governed capabilities in the core, demonstrated rollback, measured cost delta, handover pack.

**Acceptance test:** an improvement is made once to a shared capability and demonstrably reaches all its consumers with no application-level work and no user noticing.

---

## Summary

| Stage | Weeks | What exists at the end |
| --- | --- | --- |
| 1. Ground truth | 1 to 3 | You know what you have and how it connects |
| 2. Organizational model | 3 to 5 | A machine can answer what you can already do |
| 3. Feedback loop | 5 to 8 | A weekly queue of reasoned proposals. **MVP complete** |
| 4. MCP layer | 7 to 10 | Claude reuses instead of rebuilding, invisibly |
| 5. First extractions | 10 to 13 | Improvements propagate, and it is proven |

Stages 1 through 3 stand alone. If work stopped after stage 3, Capital Factory would hold a verified estate map, a live organizational model, and a weekly queue of reasoned proposals, and could act on all three without the person who built them.

---

## Deliberately not promised

- Migration of all fifty-one items or extraction of all sixteen capabilities. Committing to counts before stage 1 would be selling, not planning.
- Any change to how the team builds during the ninety days. They keep shipping.
- Automatic extraction without human approval. That is a decision to make with evidence from the proposal queue.
- A multi-connector ingestion pipeline. Connectors get added one at a time, each justified by a named question the repositories cannot answer.
- Continuously running proactive agents. Weekly is the design point.

---

## Separate from this sequence

**The app layer.** Standardized PRD creation, application scaffolding, and the new-employee onboarding skill. Stage 4 builds the hook these plug into, and they get substantially easier once the model is reliable, which is the argument for doing them second rather than first.

**Station.** Standing up the environment against an empty estate. Not scoped here, because the stage 1 interviews are what should determine it. It is the strongest argument for the whole architecture, since a system that works on an organization with no GitHub and no applications is an operating model rather than a cleanup.

**Identity and systems administration.** Assigned elsewhere. This sequence hands over what it observes and is designed around whatever identity model that work produces.

---

## Open items

1. Non-labour platform budget. Rough shape is $100 to $400 a month self-hosted, or $500 to $2,000 managed.
2. Who owns the platform after the ninety days. The operator console has exactly one user and that user is currently unnamed.
3. Data policy owner, and whether prompt and response content may be stored.
4. Whether the operator console needs to serve anyone beyond the platform owner.

---

## What starts the clock

1. Read-only access across the five repositories, and a named technical contact.
2. A Station Austin introduction and a window in the first three weeks.
3. Agreement on stage 1 and its acceptance test.

Everything else can wait for evidence.

# Product Specification

Status: Proposed. Revised August 29, 2026 for the intelligence-layer architecture.
Companion documents: `architecture-decision.md`, `system-design.md`, `requirements.md`, `90-day-proposal.md`.

---

## What this specifies

`requirements.md` states what the system must guarantee. `system-design.md` states how the pieces fit. This document states what the product actually is: who or what uses it, what job they are doing, what the interface is, and how anyone can tell whether it worked.

The short version: the product is a machine-readable understanding of Capital Factory, reachable by Claude, kept current by a weekly loop, with one small screen for the person who owns it.

---

## Product thesis

> **Reuse is a property of how an application gets planned, not a choice someone makes.**

The estate is organized by person because copying was the only affordance the system offered. Six of the ten duplicated capabilities are duplicated once per teammate. Nobody did anything wrong.

The fix is not a better place to shop. It is to make the question "do we already do this?" get asked automatically, by the thing already planning the application, before anyone writes a line. A builder who never learns the understanding layer exists, and whose application quietly runs on three existing capabilities, is the success case.

The SDK analogy, taken literally: SDK users do not browse the source. They call it, and it is good or it is not.

---

## Consumers

Ordered by frequency of interaction. The order is the point.

| Consumer | What it is | What it needs |
| --- | --- | --- |
| **Claude at plan time** | The model decomposing a plan into work | Ask what exists, get a trustworthy answer with a usage example, wire it up |
| **The weekly loop** | A scheduled job | Read the estate, write findings and a regenerated model back |
| **Platform owner** | One named person, currently unassigned | Approve or reject proposals, see coverage and drift, know what the layer is failing to answer |
| **Capability owner** | Whoever owns an extracted capability | Know who consumes it, release safely, see when an evaluation fails |
| **Executive sponsor** | Reads occasionally | Is this working, and what does it cost |
| **Builders** | The eight people writing applications | Nothing. By design. They describe what they want and say go |

The last row is a requirement, not an omission. It is also the single easiest thing to violate by accident, because every instinct in product design pulls toward giving people a screen.

---

## Object model

- **Application** - a job owned end to end. Where it lives, who owns it, what it consumes, whether it is live.
- **Capability** - one reusable competency. May live inside an application or in the core.
- **Tool** - how a capability reaches the outside world: MCP server, API, or custom integration.
- **Deployment** - where an application actually runs, including employee machines.
- **Data source** - a system of record.
- **Contract** - what a capability does, what it never does, inputs, outputs, systems touched, approval gates.
- **Consumer edge** - application X depends on capability Y at version Z. The edge is what makes a breaking change detectable.
- **Similarity edge** - A and B appear to do the same thing. Proposed, never assumed.
- **Proposal** - one finding with one recommendation and its reasoning.
- **Query record** - what was asked, what matched, what got built.
- **Evaluation** - the check that gates entry to the core and every release after.

---

## Jobs to be done

Ranked by duplication prevented, not by how often a human touches them.

1. **"Of the twenty things this plan needs, which already exist?"** Asked by Claude, answered by the model, invisible to the builder. The only job that prevents cost rather than recovering it.
2. **"What got built twice this month, and does any of it deserve to move?"** The weekly loop, with reasoning attached to every answer.
3. **"Is it safe to change this?"** Consumers, versions in use, evaluation status, before release.
4. **"What did we fail to answer?"** Non-matches, which predict the next duplication before it happens.
5. **"Where would a cheaper model be good enough?"** Per-function routing, possible only once functions live in one place.
6. **"Did last week's proposals go anywhere?"** The failure mode, made visible.
7. **"Why did we decide that?"** Decision records, readable by someone who was not in the room.

---

## Surfaces

Three. Two of them have no screen, and those are the two that matter.

### 1. The organizational model

No screen. Generated, never authored.

- `REG-001` (v0): Generate an entry for every capability directly from source across the five repositories.
- `REG-002` (v0): Each entry carries what it does, what it never does, inputs, outputs, systems touched, how it is invoked, owner, consumers, evaluation status, and extraction status.
- `REG-003` (v0): Descriptions are written for a model to read, and their quality is tested rather than proofread.
- `REG-004` (v0): Consumer edges recorded, so "who does this affect" is answerable instantly.
- `REG-005` (v0): Freshness stamped per entry.
- `REG-006` (v0): Stored in a format that ports if a first-party registry ships.
- `REG-007` (v1): Live-versus-dormant status.
- `REG-008` (v0): Valid when empty, so the architecture can be stood up at an organization with no estate yet.

### 2. The MCP layer

No screen. This is the product's primary interface.

- `MCP-001` (v1): `capability_search(description)` returns candidates matched on described behaviour, with confidence and a usage example.
- `MCP-002` (v1): `capability_get(id)` returns the full contract and how to invoke it.
- `MCP-003` (v1): `application_list(filter)` returns what exists, who owns it, what it consumes.
- `MCP-004` (v1): `standards_get(context)` returns the conventions a new application should follow.
- `MCP-005` (v1): `systems_describe(name)` returns what a system of record holds and the rules for writing to it.
- `MCP-006` (v1): `usage_log(query, matches, outcome)` records what was asked and what was built.
- `MCP-007` (v0): Metadata only. The layer never proxies data held in systems of record. This is a hard boundary, not a phase-one simplification.
- `MCP-008` (v1): Distributed to builders' Claude environments without per-person setup work.
- `MCP-009` (v1): Confidence threshold below which a match is suppressed rather than offered, because a wrong match is more expensive than no match.

### 3. The operator console

The only screen. One user.

- `CON-001` (v0): This week's proposals with reasoning, and the disposition of every prior week's.
- `CON-002` (v0): What exists, who owns it, what it touches.
- `CON-003` (v0): Coverage gaps surfaced without filtering for them: no owner, no evaluation, laptop runtime, weak description.
- `CON-004` (v0): Duplication and divergence as a trend over weeks, not a snapshot.
- `CON-005` (v1): Query telemetry. What is being asked for, found, and missed.
- `CON-006` (v1): Cost by application and capability, and where a cheaper model would do.
- `CON-007` (v1): Capability detail: contract, consumers and versions, evaluation results, release history.
- `CON-008` (v0): Decision records with status and, for anything blocked, what it is blocked on and who unblocks it.
- `CON-009` (never): A browse-and-install experience for builders. Listed explicitly so it does not creep back in.

---

## Non-goals

- Not a builder-facing marketplace. The invisibility constraint forbids it.
- Not a chat interface. Everyone already has Claude, and building a second one is the mistake this architecture exists to avoid.
- Not an identity, SSO, or credential product.
- Not a replacement for GitHub or Vercel. The system of execution does not move.
- Not enterprise search. This catalogs capabilities, not documents.
- Not a data gateway. The layer describes capabilities and never brokers the data they touch.
- Not a multi-connector ingestion platform, at least not first. Connectors get added one at a time, each justified by a named question.
- Not continuously running. Weekly is the design point.
- Not a place where capabilities are authored. It describes and governs; capabilities are written in the repository.

---

## Release phases

**v0, roughly week 8. The MVP.** The organizational model, generated. The weekly loop producing a ranked, reasoned proposal queue. The console showing that queue, the model, coverage gaps, and decisions. No core, no extractions, no application changed.

This ships as a complete product even though it changes nothing, because the queue is a standing artifact anyone can act on and because it is the instrument that says which extractions are worth doing.

**v1, weeks 8 to 13.** The MCP layer, which is where duplication actually stops. Semantic search. Query telemetry. Capability detail. Cost and routing views. The first three extractions, capped at three because extraction rate is limited by evaluation coverage.

**Later.** Predicted duplication from repeated non-matches. Automatic extraction, decided from evidence in the queue. The app layer: plan generation, scaffolding, onboarding, proactive agents.

The MCP layer is deliberately v1 rather than v0 because it depends on a model that is trustworthy. Shipping it against a model nobody trusts would poison it permanently, and a false negative is invisible at the moment it happens.

---

## Success metrics

Baselines from the July 22 survey, all reported and unverified until the estate is mapped.

| Metric | Baseline | Direction |
| --- | --- | --- |
| Capabilities implemented more than once | ~10 | Down |
| Capabilities with a drifted duplicate | at least 1 | To zero |
| Applications writing the investor database without the guardrail | 1 of 4 | To zero |
| Items with an evaluation | 1 of 51 | Up. The binding constraint on everything else |
| Model entries a model can correctly reason about | unmeasured | To 100% |
| Plans where search returned a usable match | n/a | Up, once v1 ships |
| Proposals actioned within two weeks | n/a | Up. The early warning for the whole design |
| Time from plan to first working application | unmeasured | Must not increase |

Two of these carry more weight than the rest.

**Evaluation coverage is the binding constraint.** One suite across fifty-one items caps how fast anything can safely be promoted, regardless of what the loop finds.

**Time to first working application must not increase.** If the layer makes building slower, the architecture is wrong for this team, and the honest response is independent applications with periodic audit.

Adoption is deliberately not a metric. Nobody chooses a capability, so nobody can be measured on choosing one.

---

## Open questions

- How the fifty-one items reach their systems today. Everything downstream depends on it.
- Whether the managed plugin marketplace can carry distribution for extracted capabilities.
- Whether prompt and response content may be stored, which gates telemetry depth. Needs a named data-policy owner.
- Who the platform owner is. The console has exactly one user and that user is unnamed.
- Whether an empty-estate deployment changes any v0 requirement.

## Evidence status

Counts and duplication findings are reported from the July 22 architecture survey and are unverified. Design constraints are verified from the August 26 discovery session. Everything else is proposed. The mockup contains illustrative data and describes no running system.

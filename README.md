# Capital Factory AI Systems Blueprint

Status: Working draft. Revised August 29, 2026 following the discovery session of August 26.
Prepared by: Jorge Alejandro Diez

## What this is

A worked architecture for making reuse the default outcome of building an application at Capital Factory, without anyone having to think about it.

The estate has roughly fifty-one packaged items across five repositories, thirty-one applications, about sixteen capabilities, ten of them implemented more than once, and one evaluation suite. The analysis was already done in-house. The gap is not analysis. It is that nothing asks "do we already do this?" before someone builds it.

## The shape of the answer

**The system of execution does not move.** GitHub is GitHub. Vercel is Vercel. Applications stay deployed where they are. Nothing migrates and nobody stops shipping.

**A system of understanding gets built underneath it.** A structured model of what Capital Factory has, exposed to Claude through MCP, kept current by a weekly loop that learns from every new application.

Four properties, in the order they were decided:

1. **Map what exists.** Repositories, skills, APIs and MCPs, deployments, data sources, turned into a structured understanding of the capabilities already built.
2. **Make it available to Claude.** An MCP layer, so that before building something new, Claude checks what already exists, what can be reused, what standards apply, and where the relevant systems live.
3. **Close the loop.** As applications get built, the system learns from them, finds duplication and drift, and keeps the shared understanding current. Every new application should make the next one easier.
4. **Keep it invisible.** Employees keep working through Claude and the tools they already use. Nobody browses a catalog, because nobody should have to know the layer exists.

## The three decisions

Set out in full, with reasoning and reversal conditions, in `architecture-decision.md`.

**What do we do with a duplicate?** Extraction and audit are not competing approaches. Extraction decides where code lives; audit decides how you find out something should move. The rule: extract when a change must propagate, leave it and audit when a change must not. Four criteria decide each case.

**How does the answer reach the builder?** Not through a new interface. Claude is already the interface, so Capital Factory becomes something Claude can query. The layer serves metadata about capabilities and never the data those capabilities touch, which is both the right boundary and what makes it deliverable.

**How does the layer learn what exists?** By reading the repositories first, and adding a connector only when a named question cannot be answered without one. A multi-connector ingestion pipeline is the part of this system most likely to be commoditized within two model releases, and it is the part with nothing to connect to at Station.

## The design rule underneath

> Keep the machinery thin, standard, and disposable. Make the organizational context durable and compounding.

Cross-repository understanding, duplicate detection, and skill packaging will very likely be commoditized. What Capital Factory knows about itself will not be. The model's content is the asset; every piece of machinery that reads or serves it should be built to be deleted.

## Documents

| Document | Purpose |
| --- | --- |
| [architecture-decision.md](architecture-decision.md) | The three decisions, with trade-offs and the conditions that would reverse each |
| [system-design.md](system-design.md) | The system that follows: model, MCP surface, weekly loop, operator console |
| [prd.md](prd.md) | What the product is: consumers, jobs, surfaces, release phases, metrics |
| [requirements.md](requirements.md) | What the system must guarantee, with identity and security preserved in an annex |
| [90-day-proposal.md](90-day-proposal.md) | Five-stage implementation sequence, the MVP, and an acceptance test per stage |
| [discovery.md](discovery.md) | What is still unknown, what the session answered, and what moved out of scope |
| [technical-stack.md](technical-stack.md) | Platform posture, rejected alternatives, candidate components |
| [prior-art.md](prior-art.md) | Survey of platforms that solve part of this, and why none is adopted wholesale |
| [mockup.html](mockup.html) | Working concept of the operator console. Illustrative data only |
| [deepresearch.md](deepresearch.md) | Background on Capital Factory, its portfolio, and its market position |
| [styleguide.md](styleguide.md) | Digital style guide |
| [voice.md](voice.md) | Writing voice |
| [sources/README.md](sources/README.md) | Register of materials used |

## Evidence labels

- **Verified** - confirmed through an authoritative system, repository, responsible owner, or directly in the discovery session.
- **Reported** - stated in Capital Factory materials but not independently confirmed.
- **Proposed** - a recommendation for discussion.
- **Unknown** - information still required.

Unless marked verified or approved, architecture and technology selections here remain provisional.

## Vocabulary

Capability rather than skill. The core. The middle layer. Like an SDK. These are Capital Factory's own words and they are used in preference to ours.

## Scope boundaries

This is not an approved production specification, not the future shared-core source repository, not a complete inventory, not authorization to access any system, and not a commitment to any vendor.

It does not cover identity, SSO, or systems administration, which belong to a separate IT and security engagement. The relevant requirements are preserved in the annex to `requirements.md` so that whoever takes that work starts from evidence rather than from zero.

It does not propose anything for Station. Standing the environment up against an empty estate is the strongest argument for this architecture, but what Station needs should come from talking to Station.

## The first thing to find out

How the fifty-one items actually reach the systems they touch. In many cases MCPs, in some cases an API or something custom built, and the distribution is currently unknown.

If it is a handful of shared MCP servers, the understanding layer has a clean seam to sit on and extraction is largely packaging. If it is thirty-one hand-rolled integrations, this is a different project. Everything estimated downstream of that question is labelled as an estimate for exactly this reason.

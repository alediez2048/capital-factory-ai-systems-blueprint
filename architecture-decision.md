# Architecture Decisions

Status: Recommendation for Capital Factory review
Prepared by: Jorge Alejandro Diez
Date: August 29, 2026
Source of the questions: Gordon Daugherty, call of August 26, 2026
Companion: `gordon-call-01-notes.md` (primary source), `system-design.md` (what gets built)

---

## What this document is

Gordon asked one question directly and, in the way he asked it, implied two more. All three have to be answered before anything can be estimated honestly, and they have to be answered in order, because each one narrows the next.

**Decision 1.** When we find the same capability built three times, what do we do about it? Extract into a core, or leave the applications alone and audit for drift?

**Decision 2.** However that gets answered, how does the answer reach the person building application 52, given that they must never have to think about it?

**Decision 3.** How does the system learn what Capital Factory already has? By reading the repositories, or by ingesting everything Capital Factory produces into a model of itself?

Decision 1 is the one Gordon asked. Decision 2 is where the design became concrete. Decision 3 is where this proposal is most likely to go wrong, and it is stated here so it can be argued with rather than discovered in month two.

His instruction, twice:

> "Ignore the approach that I was thinking we would take unless you conclude it is a viable approach."

What follows does not agree by default and does not disagree for effect.

---

# Decision 1: extraction or audit

## The question as he asked it

> "Another approach would be to not pull things into a core. Leave heavy apps heavy, let them full load everything they need, and instead build an auditor agent that once a week or once a month runs, and it tries to discover drift and best practices. 'Hey Gordon, Nick did something amazing that makes his whatever faster and more secure. Would you like to compare it to yours?' It's a different approach that I hadn't thought about."

## Short answer

Neither alone. The two are not alternatives at the same layer, and treating them as a choice is what makes the decision look hard.

**Extraction is a decision about where code lives. Audit is a decision about how you find out that something should move.** Every real version of this system needs both. What actually has to be decided is narrower:

> When the auditor finds the same capability implemented three times, does it propose one merge into the core, or three patches to three applications?

That answer is different per capability, and it turns on which of two properties matters more: **propagation** (one change reaches every consumer) or **independence** (one application's change cannot affect another).

The rule:

> **Extract a capability into the core when a change to it must propagate. Leave it in the application and audit it when a change to it must not.**

Roughly ten of Capital Factory's sixteen capabilities are duplicated. On the evidence available, a minority meet the propagation test. Most of the value in the first ninety days comes from three or four extractions, not from a migration programme.

## What each model is optimizing

### Extraction

What it buys, using Gordon's own example, which is the strongest argument anyone has made for it:

- **Propagation.** Add SEC and PitchBook to the startup-ingestion capability, and the investment-decision application, the mentor-matching application, and the co-founder-referral application all inherit it with no application work and no user awareness.
- **Per-function model selection.** With five analysis functions in one place, each routes to the cheapest model that is good enough, re-evaluated when a new model ships. In three copies that optimization has to be found and executed three times, which means it never happens.
- **One place to enforce a control.** The safe write to the investor database is either in the capability or it is a convention. Today it is a convention, and the fourth application does not follow it.

What it costs:

- **Coupling.** A shared capability with three consumers has three ways to break, and whoever changes it now owns a blast radius they did not have before.
- **A release process where there was none.** Versioning, deprecation, consumer upgrades. Real overhead on a team of eight who currently move by copying a file.
- **Premature abstraction.** Two similar implementations are not always one capability. Sometimes they are two that rhyme, and forcing them together produces a parameterized thing that serves neither.

### Audit

What it buys:

- **No coupling, ever.** Nick's application cannot break Jamie's. For eight builders at different skill levels with no platform team, that is worth more than it sounds.
- **It respects the constraint he stated.** No migration, no freeze, no slowdown.
- **It degrades gracefully.** Switch off the auditor and nothing breaks. Switch off a shared core and thirty-one applications break.
- **It is cheap to build and cheap to be wrong about.** A read-only agent over five repositories, run weekly, is a two-week build with almost no downside.

What it costs:

- **Propagation becomes manual and therefore optional.** The auditor can say Nick's version is better. Somebody still has to open three pull requests, and on a team with no platform owner that somebody is nobody.
- **It cannot enforce a control.** A control that depends on a human reading a weekly digest is not a control.
- **It answers "do we already do this?" too late.** By then the duplicate has shipped.

## Where it lands

The auditor's fatal flaw is that its output needs human follow-through. The core's fatal flaw is that its input needs human effort up front. They fail at opposite ends of the same pipeline.

The asymmetry that decides it: an auditor without a core produces proposals with nowhere to land. Its best finding is "these three diverged," and its only remedy is three patches, which is the work that created the divergence. It is a smoke detector in a building with no fire exits. A core without an auditor still works; it just grows more slowly.

So the core is load-bearing and the auditor is the discovery mechanism that feeds it. That is Gordon's own conclusion, adopted for this reason rather than because he reached it first, with two corrections his framing does not include.

**The auditor is not a later nicety, it is how the core stays honest,** and it should run before the first extraction, because its first job is to tell you which extractions are worth doing. Build the instrument before you operate.

**The default is not to extract.** The auditor's finding is an input to a test, not an extraction ticket.

## The extraction test

All four, or it stays where it is.

**1. Propagation is required, not merely nice.** Would a future improvement need to reach every consumer without asking them? The data-source example passes. Drafting in Jamie's voice fails, because Drew does not want Jamie's improvements.

**2. There is a correctness or compliance stake.** Does a wrong implementation write bad data, leak something, or break a rule? The investor-CRM safe write passes on this alone.

**3. The implementations are genuinely the same thing.** Same inputs, same contract, same success condition. If unifying them takes more than two configuration flags, they are two capabilities.

**4. It has, or can be given, an evaluation.** A shared capability without an evaluation is a shared liability: three applications' risk concentrated into one component with no way to know when it regressed.

Criterion four is deliberately the binding one. Capital Factory's survey found one evaluation suite across fifty-one items. That number, not the duplication count, sets how fast the core can grow, and it makes the ninety-day plan honest about volume.

---

# Decision 2: how the answer reaches the builder

## The constraint

> "Me as a user, I don't want to know about the core. I don't want to know about the skills that are out there. I don't want our employees to have to worry about what's in the core."

This kills the design most people reach for first, including the one in the earlier version of this proposal: an internal catalog where builders browse capabilities and pick what they want. If nobody browses, the browse experience is not a feature that goes unused. It is a wrong answer.

But it leaves a hard question. If the builder never opens anything, and never learns the registry exists, what actually asks "do we already do this?" and when?

## The answer

Gordon already described the moment:

> "The following twenty things, tell me if we already did that before. Because if so, I am not going to rewrite it. I am just basically going to connect to that core capability that's already there."

The moment is plan time, and the thing asking is Claude. So the interface is not a screen, and it is not a chat window either. **Claude is already the interface. Capital Factory does not need to build one, it needs to become something Claude can query.**

Concretely: Capital Factory exposes its own understanding of itself as an MCP server. Claude Code already speaks MCP. A builder opens Claude the way they already do, describes what they want, and Claude calls the tools without being asked to.

```
Capital Factory employee
        |
        v
      Claude              (already installed, already how they work)
        |
        | MCP tool calls
        v
Capital Factory understanding layer
        |
        +-- what capabilities exist
        +-- where each one lives and who owns it
        +-- what systems it touches
        +-- what standards a new application follows
        |
        v
GitHub, Vercel, existing MCPs, systems of record
```

## Why this is the right answer and not just a clever one

**It satisfies the invisibility constraint structurally, not by discipline.** A catalog that people are told not to open still gets opened, or worse, gets ignored on the days it matters. A tool call that happens inside planning cannot be skipped and cannot be seen.

**It avoids building another destination.** Capital Factory's people already live in Claude, GitHub, Vercel, and the applications themselves. A new internal platform is a fifth place, and a fifth place with one part-time owner is a place that goes stale. The estate does not have a UI problem.

**It separates two things that were tangled.** The **system of execution** stays exactly where it is: GitHub is GitHub, Vercel is Vercel, applications stay deployed where they are deployed. What Capital Factory gains is a **system of understanding** layered underneath, answering what we have built, what we can already do, where it lives, who owns it, and what a new application should follow. Nothing moves. Nothing migrates.

**It makes onboarding fall out for free.** Gordon's Phase 2 onboarding skill becomes configuration rather than construction: a new employee's Claude gets the Capital Factory MCP, and they inherit the institutional knowledge without anyone walking them through fifty applications.

**It is the right shape for the obsolescence problem.** MCP is a standard interface, not our invention. If Anthropic ships a first-party capability registry, the content ports and only our server implementation is discarded. That is a good day.

## What this does not solve, and should not pretend to

The MCP server exposes **metadata about capabilities**, not the data those capabilities touch. It says the estate can analyse a startup and where that lives. It does not read the investor database. That boundary is deliberate: the moment the understanding layer proxies real data, it inherits an access-control problem, and identity is explicitly assigned to a separate IT and security engagement. Keeping the layer metadata-only keeps it out of that dependency, which is also what makes it shippable in ninety days.

If it later needs to broker access to data, that is a decision to make with the IT consultant in the room, not a thing to slide into scope.

---

# Decision 3: how the layer learns what exists

This is the decision most likely to sink the engagement, and it is the one nobody asked, which is why it is stated here.

## The two options

**A. Read-through.** The understanding layer reads the five repositories directly, on a schedule, and generates the registry from what it finds. A script and a scheduled job.

**B. Ingest and model.** Connectors across GitHub, Vercel, existing MCPs, APIs, documents, PRDs, skills, and deployments, feeding raw events into normalization, then AI enrichment, then a persistent organizational model that the MCP server serves from.

Option B is the more impressive architecture and it is what a well-funded platform team would eventually build. It is the wrong first move here, for four reasons.

## Why read-through wins, for now

**The premise behind ingestion may not hold.** Ingestion pipelines exist to turn messy operational exhaust into structure. Capital Factory's estate is fifty-one Claude Code skills in five GitHub repositories, forty of them reported as strongly documented. That is markdown in git. It is already structured, already versioned, and already readable in one pass. Building a streaming pipeline for it is solving a problem the evidence does not show.

**It fails Gordon's own obsolescence test.** An eight-connector ingestion, normalization, and enrichment pipeline is precisely the undifferentiated plumbing he said he is afraid of investing in:

> "Not spend a lot of time and effort on some things today that are very likely to just be automatic within the Claude capability set in three months or six months."

Cross-repository understanding is the single most likely thing to be commoditized next. Building a pipeline for it is spending the quarter on the part that expires.

**It is not deliverable at this size.** Connectors, event store, normalization, enrichment, model, API, MCP server, and auditor is team-quarters of work. This engagement is one person at twenty to thirty-five hours a week for ninety days. Proposing it would be selling, and the failure would land in month two.

**It works at Station and the pipeline does not.** Station has no GitHub. A connector-first design has nothing to connect to there, which breaks the strongest strategic argument in the whole engagement: build the model at Capital Factory, port the environment to Station. A registry-first design works at Station on day one, with zero entries, which is the correct and useful answer for an organization that has not started yet.

## The decision

> **Read the repositories directly. Add a connector only when a specific question cannot be answered without it, and name the question first.**

The likely first exception is deployment reality. Whether an application is live, and where it runs, is not reliably in the repository, and two items are reported to run on someone's laptop. That is one connector, justified by one question, added when the question becomes blocking.

This is not a rejection of the ingestion architecture. It is a sequencing decision. The organizational model is the durable asset either way; how it gets populated is an implementation detail that should start as cheaply as it can and grow only against evidence.

---

# The design rule underneath all three

Gordon's fear, and he is right to have it:

> "There is a risk that I put all this energy into defining this nice core and six months from now Claude just does it automatically."

Some of that will happen. The way to not be hurt by it is to be deliberate about which side of the line each piece sits on.

**Anthropic will very likely commoditize:** cross-repository code understanding, duplicate detection, refactoring proposals, skill packaging and distribution, and probably much of the sweep's analysis.

**Anthropic will not build:** what a good startup analysis looks like at Capital Factory specifically, which sources are trusted, which decisions need human approval, who owns what, what "done" means for a mentor match.

So:

> **Keep the machinery thin, standard, and disposable. Make the organizational context durable and compounding.**

The registry's content is the asset; its implementation is not. The extraction test is the asset; the agent applying it is not. The MCP server is an interface everyone already speaks, which is exactly why it should be ours to serve and not ours to invent.

---

# What would change these recommendations

Stated so they can be checked rather than trusted.

**Extract less, audit more, if** the discovery audit finds fewer than four duplicated capabilities passing all four criteria. Then the core is not yet worth building and the deliverable is the registry, the MCP layer, and the auditor, with extraction deferred.

**Extract more aggressively, if** the tool layer turns out to be uniform, with three or four shared MCP servers underpinning most of the estate. Then extraction risk is much lower than assumed and the first tranche can be six or eight.

**Build the ingestion pipeline, if** reading the repositories leaves specific, named questions unanswerable, and those questions are blocking real decisions. One connector at a time, each justified by its question.

**Abandon our registry implementation, if** Anthropic ships a first-party capability registry with a plan-time hook that reads Capital Factory's existing format. Then adopt it, port the content, and spend the remaining time on evaluations and the extraction test, which are the parts nobody else will write.

**Reverse to heavy applications and audit only, if** after the first three extractions the team reports the shared layer slowed them down. This is measurable: time from PRD to first working version, before and after. If it moves the wrong way, the core is wrong for this team and the auditor-only model was the right answer.

---

# The one thing that has to be learned first

All three decisions rest on a question nobody can currently answer, and Gordon said so himself:

> "In many cases it's MCPs, some cases it's maybe an API or something custom built. I don't know."

Until the tool layer is mapped, the extraction estimates here are informed guesses. Mapping it is the first milestone for that reason. If the fifty-one items reach their systems through a handful of shared MCP servers, extraction is mostly a packaging exercise and the understanding layer has a clean seam to sit on. If they reach them through thirty-one hand-rolled integrations with embedded credentials, this is a different project, and the honest thing is to say so in week three rather than month three.

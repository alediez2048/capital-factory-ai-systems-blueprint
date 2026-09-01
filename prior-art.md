# Prior Art: Platforms That Already Solve Part of This

Status: Market survey supporting ADR-009. All vendor claims are `Reported` from vendor documentation, pricing pages, and third-party analysis; none are independently verified.
Last updated: August 20, 2026
Question this document answers: before Capital Factory builds anything, is someone already selling or giving away a governed catalog for internal AI capabilities, and should we buy it instead?

## Why this document exists

`technical-stack.md` proposes assembling a thin control plane rather than adopting an end-to-end platform. That recommendation is only credible if the alternative was genuinely searched for rather than waved away. This document is that search: open-source and commercial products whose pitch overlaps with what Capital Factory needs, what each actually delivers, and why the recommendation survives.

The short answer: the product category Capital Factory needs exists, it is roughly eighteen months old, and every mature entrant in it is priced and shaped for organizations two orders of magnitude larger. The affordable products solve a different problem than the one Capital Factory has.

## The category confusion, and why it matters

Vendor listicles collapse four distinct product categories into "AI governance." They are not interchangeable, and only one is the category in question.

| Category | What it governs | Representative products | Relevant here |
| --- | --- | --- | --- |
| Agent registry / control plane | Which capabilities exist, who owns them, who may call them | Credal Agent Registry, AWS Agent Registry, Glean Agent Governance | Yes. This is the category. |
| LLM observability and evaluation | What happened at runtime: traces, cost, eval scores | Langfuse, Vellum, Galileo, Orq.ai, Maxim, Arize | Partly. Solves half of one requirement. |
| AI regulatory and model-risk governance | Bias, drift, model cards, EU AI Act evidence | Credo AI, Holistic AI, IBM watsonx.governance | No. Wrong problem entirely. |
| Agent builders and vertical agent products | Building new agents in the vendor's own runtime | Sierra, Moveworks, Agentforce, Relevance AI, Lindy, Stack AI, Gumloop | No. Sells a place to rebuild, not a place to catalog. |

The third category is the most common false positive. A search for "AI governance platform" returns products built for banks and insurers proving model fairness to regulators. Capital Factory's problem is that ten capabilities exist in three copies each. These are unrelated problems solved by unrelated software.

## Category 1: The products that actually match

### AWS Agent Registry (Amazon Bedrock AgentCore), Preview since April 2026

The closest literal match found anywhere. A managed, searchable, private catalog for publishing and discovering agents, MCP servers, tools, and, notably, `agent skills` as a first-class registered resource type. Ships with publisher, curator, consumer, and administrator roles, an approval workflow before a resource becomes discoverable, IAM or JWT authentication, CloudTrail audit logging, and EventBridge notifications on registry events.

Read that feature list against `requirements.md` and it maps almost line for line onto GOV-001 through GOV-004 and CORE-001 through CORE-004. It is the only product surveyed that treats "skill" as a noun the system understands.

Why it is a watch item rather than a recommendation: it is in Preview, it is AWS-only, and Capital Factory's estate is Google Workspace, GitHub, and Vercel. Adopting it means introducing AWS as new infrastructure solely to host a catalog, and it is an assembly-required primitive rather than a product a non-engineer opens. Preview status also means no pricing signal and no stability commitment.

Verdict: track it. If Capital Factory ever has an AWS presence for another reason, this becomes the default answer and ADR-009 should reopen.

### Credal AI Agent Registry

Sells exactly the pitch: a governed registry that stops agent sprawl and duplication, with ownership, verified-versus-draft status, named-owner approval chains, role-scoped publish rights, and an audit trail of changes and calls, plus usage and cost dashboards.

Why not: no published pricing, enterprise sales motion only, and the reference deployments shown are registries of 128-plus agents. It also wants Capital Factory's capabilities to be Credal agents, not GitHub-resident Claude Code skills. Buying it means re-authoring the estate into a vendor runtime, which is the format trap described below.

Verdict: worth one exploratory call if only to see the product, but the shape and likely five-figure floor do not fit an eight-builder team.

### Glean Agent Governance

Real capability, wrong purchase. Glean's agent library and governance module sit on top of Glean's enterprise search platform, and third-party transaction data puts the base platform at a median near $99,000 per year with a floor around $30,000. Buying enterprise search to obtain an agent catalog is backwards for a 25-person firm.

## Category 2: Open source, examined honestly

Thirteen platforms were surveyed. None is a skill catalog. They cluster into three shapes.

**Chat interfaces with sharing bolted on.** LibreChat (MIT, roughly 40k stars) is the strongest: multi-user authentication, admin panel with groups and roles, an agent marketplace for sharing internally, and native MCP support. Onyx (MIT core, YC-backed, roughly 32k stars) is the best-connected to Capital Factory's actual stack, with native GitHub and Google Workspace connectors, custom assistants, usage graphs by team and agent, and SSO plus RBAC in its paid Enterprise edition. Open WebUI, AnythingLLM, and Lobe Chat are lighter variants of the same idea. All of them share one limitation: the unit of reuse is an assistant, not a versioned capability with a contract and a release gate. They are catalogs of chatbots, not catalogs of capabilities.

**Visual workflow builders.** Dify (roughly 100k stars) has the most credible reuse story in this group, publishing workflows as tools that other apps call, plus a plugin marketplace. Langflow and Flowise are larger and thinner respectively on governance. n8n is mature and widely deployed. Two problems: all are drag-and-drop first, which fits a code-first team badly, and in Dify's and n8n's cases the governance features that matter, RBAC, environments, and Git-backed versioning, sit behind paid Enterprise tiers. n8n's Community edition explicitly restricts workflow access to the instance owner and creator, which is not a permission model.

**Code-first agent frameworks.** Agno (Apache 2.0, roughly 42k stars) is the standout, shipping JWT-based RBAC, multi-tenant isolation, OpenTelemetry tracing, and audit logs inside an open-source runtime. CrewAI and Letta are libraries for building agents rather than catalogs for governing them, and CrewAI's governance lives in its paid Enterprise product. These are things you build on, not things that ingest an existing estate.

**Not AI-specific but architecturally correct.** Backstage implements exactly the pattern Capital Factory wants: YAML-defined catalog entities, ownership, Git-backed versioning, a discovery interface. It is not AI-aware, and the AI and MCP plugin ecosystem around it was not mature as of August 2026. It remains the reference model for what a catalog is, which is why the thin GitHub-native catalog proposed in the stack document borrows its shape.

## The finding that decides it: the format trap

Every platform surveyed, open source and commercial alike, defines capabilities in its own schema. Dify uses its DSL, Flowise and Langflow use JSON graphs, LibreChat and Onyx use their own assistant configs, Agno and CrewAI use Python classes, Credal and Glean use their own agent objects.

None of them ingests Claude Code's skill format. Adopting any one of them means re-authoring 51 items into a proprietary format that Capital Factory does not control and cannot easily leave.

That is not a migration cost. It is the original problem, recreated. The firm's documented failure mode is capabilities scattered across formats and locations with no canonical source. Answering that by moving everything into a vendor's format, where the exit cost is total, makes the estate less portable, not more. The one exception is Anthropic's own private plugin marketplace, which distributes the format the team already writes, which is exactly why the stack document keeps it as a spike and everything else as prior art.

## MCP as the portability hedge

The interoperability layer nearly every surveyed platform now speaks is MCP. LibreChat, Onyx, Open WebUI, Dify, Langflow, and the AWS Agent Registry all consume MCP servers, as does Claude Code itself. There is now an official MCP registry, a Docker MCP catalog, and published patterns for running a private enterprise registry.

The practical consequence for Capital Factory: wrapping the roughly ten duplicated capabilities as internal MCP servers once makes them callable from Claude Code today and from any of these platforms later, without committing to any of them now. That is the cheapest available insurance against the format trap, and it is compatible with every route in ADR-009. It belongs in the ADR-001 evaluation alongside the marketplace spike.

## Cost reality

The products with real self-serve pricing solve observability, not cataloging. Vellum publishes tiers from $30 to $200 per month. Galileo publishes a free tier and $100 per month for Pro with RBAC. Orq.ai publishes a pay-as-you-go model with per-seat and per-span pricing, though audit logs and on-premise deployment are enterprise-tier. Langfuse self-hosted is free with unlimited traces, projects, and users, with project-level RBAC and audit logs licensed.

The products that catalog and govern do not publish pricing at all. Credal, Airia, Glean agents, Moveworks (reported median near $130,000 per year with a floor near $50,000), and ServiceNow's AI Control Tower are all quote-only, enterprise sales motion.

So the market offers Capital Factory cheap observability or expensive governance, and nothing that is both, at this scale.

## Vendor stability signals worth recording

Three data points argue against betting the estate on a single young vendor in this category. Humanloop, still listed as a live option in several 2026 comparison articles, was acqui-hired by Anthropic in August 2025 and sunset the following month. Galileo is subject to a Cisco acquisition announcement. Stack AI was acquired by Asana in May 2026. Category churn at this rate is itself an argument for keeping capabilities in a portable format and treating any platform as a replaceable layer.

## What this means for ADR-009

The survey strengthens the recommended posture rather than changing it, and it sharpens two of its clauses.

The posture stands: assemble commodity layers, build the thin Capital Factory-specific layer, keep capabilities in a format the firm controls. Nothing found solves the catalog and reuse problem at this scale without imposing a proprietary format.

Two amendments follow. First, add internal MCP packaging to the ADR-001 evaluation as the portability hedge described above. Second, add the AWS Agent Registry to a formal watch list with a named reopening condition: if Capital Factory adopts AWS for any other reason, or if the registry exits Preview with pricing and non-AWS ingestion, the buy decision should be reconsidered on its merits.

What would have changed the recommendation, recorded so the reasoning is falsifiable: a product that ingests existing Claude Code skills without re-authoring, published pricing under roughly $500 per month, real per-capability permissions and audit, and a credible exit path. No product surveyed meets all four. Three meet none.

## Products surveyed

Agent registry and control plane: Credal AI, Glean Agents, AWS Bedrock AgentCore Agent Registry, Airia, ServiceNow AI Agent Orchestrator and AI Control Tower.
Observability and evaluation: Langfuse, Vellum, Galileo, Orq.ai, Maxim AI, Arize Phoenix, Aporia, Humanloop (defunct).
Regulatory and model-risk governance: Credo AI, Holistic AI, IBM watsonx.governance.
Open-source platforms and frameworks: Dify, Flowise, Langflow, LibreChat, Open WebUI, Agno, CrewAI, Letta, AnythingLLM, Lobe Chat, Onyx, Rivet, n8n, Backstage.
Agent builders and vertical products: Writer, Sierra, Salesforce Agentforce, Moveworks, Stack AI, Relevance AI, Lindy, Gumloop.

## Method and limitations

Desk research conducted August 20, 2026 across vendor documentation, published pricing pages, GitHub repositories, and third-party comparisons. No product was installed, trialed, or demonstrated. Pricing for quote-only products is inferred from third-party transaction data and should be treated as indicative only. Feature claims are as published by vendors and are not independently verified. This document should be refreshed before any procurement decision, and its conclusions revisited if the estate grows substantially or headcount changes the per-seat arithmetic.

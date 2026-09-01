# Prior Art: What Already Exists, and What Does Not

Status: Market survey supporting the build recommendation.
Revised September 2026. Supersedes the earlier survey, which was broader but framed the question less usefully.

---

## Why this document exists

Before recommending that Capital Factory build anything, the honest first move is to go looking for the product that already does it. This records that search, what it found, and the one thing it did not find.

The short version: this is not an invented category. Three established markets are converging on almost exactly the shape described in `system-design.md`, several of the entrants are well funded, and one of them has built something very close to the internal catalog this project originally proposed. That is a real finding and it changes the recommendation, though not in the direction it first appears to.

---

## Three markets converging on one shape

### 1. Internal developer portals

Port, Backstage, Cortex, OpsLevel, Datadog.

Organize applications, owners, deployments, documentation, and operational health. Mature category, well understood, largely aimed at engineering organizations managing services.

Port is the most relevant single product in this entire survey. It now advertises an agentic software development platform with a context lake, an MCP hub, agent management, a skills registry, governance, metrics, and human approvals. That is close to the original Capital Factory mockup, and closer than anything else here to the developer-facing half of what this project describes.

### 2. Agent management platforms

ServiceNow AI Control Tower, Microsoft Agent 365, IBM watsonx Orchestrate, Kore.ai Agent Management Platform, Airia, Google Gemini Enterprise.

Inventory, govern, observe, and sometimes orchestrate AI agents.

- **ServiceNow AI Control Tower** discovers agents, models, MCP servers and other AI assets, then manages ownership, risk, lifecycle, performance and business value. It is the closest thing on the market to a complete enterprise control centre.
- **Microsoft Agent 365** provides central inventory, ownership, security, observability and lifecycle governance for Microsoft and third-party agents.
- **IBM watsonx Orchestrate** discovers, catalogs, evaluates, governs and orchestrates agents regardless of where they were built.
- **Kore.ai AMP** manages agents across LangGraph, CrewAI, AutoGen, Google, AWS, Microsoft and Salesforce, with governance, evaluation, cost and performance monitoring. One of the clearest vendor-neutral implementations of the control-plane idea.
- **Airia** covers shadow-AI discovery, agent and model cataloging, a model gateway, orchestration, runtime policy, red teaming and compliance.

### 3. AI supply chain and identity

JFrog AI Catalog, Okta for AI Agents, Drata AI Agent Governance.

- **JFrog AI Catalog** builds governed registries for models, MCP servers and reusable agent skills, with versioning, security scanning and access control. The strongest match for the shared-capability and registry portion specifically.
- **Okta for AI Agents** discovers agents, assigns human owners, treats agents as identities, and controls their permissions and lifecycle. Solves the accountability portion, which this project explicitly does not.
- **Drata** discovers agents, enforces policy, and produces continuous compliance evidence.

### The convergence is real and active

Vendors are merging the layers rather than staying in their categories. Port advertises agent management and a skills registry. ServiceNow inventories models, agents and MCP servers alongside traditional assets. The distance between an internal developer portal and an AI control plane is closing quickly.

All claims here are `Reported` from vendor documentation and public material as of September 2026 and require verification before any procurement decision.

---

## The observation that matters

Read the three categories together and one property is shared by every product in all of them.

**They govern what already exists.**

Discover the agents. Inventory them. Assign owners. Monitor cost, performance and drift. Produce evidence. Every one of those actions happens *after* something has been built.

None of these products is in the room at the moment somebody decides to build.

This is not a criticism. Post-hoc governance is a genuine and valuable job, and at enterprise scale it is the harder job. But it means a control tower cannot stop the fourth copy of a capability from being written, because by the time the fourth copy appears in the inventory, the fourth copy exists.

The same argument that rules out an internal catalog for Capital Factory rules out the entire category as a solution to the specific problem this project is trying to solve. That is a useful thing to have discovered, and it is the reason the recommendation survives.

---

## The gap

**What the market sells:** inventory, governance, observability, compliance and identity. Portfolio-scale, post-hoc, and priced and sized for organizations far larger than this estate.

**What no one sells:** prevention at plan time. Capability descriptions written to be read by a model, reachable from inside the tool a builder already has open, answering "do we already do this?" before a line is written.

That gap is small. It is a description standard, a decision rule, and a hook. Because it is small, it does not have to compete with any of the products above, and it does not have to replace them. It can sit on top of whichever one an organization already owns.

---

## What this changes about the recommendation

This survey materially narrows what Capital Factory should build, and the narrowing should be stated plainly rather than absorbed quietly.

**Capital Factory should probably not build:** its own agent catalog, its own governance engine, its own identity layer, its own observability stack, or its own generic creation platform. Those exist, they are better funded than anything this engagement would produce, and several of them would be defensible purchases.

**What remains genuinely worth building** is three things, none of which is a platform:

1. Capability descriptions written to be read by a model, and the standard that keeps them good enough to match against.
2. The rule that decides what gets shared and what stays independent, with its four criteria.
3. The plan-time hook that puts the first in front of Claude using the second.

Everything underneath those three is a scheduled job, a standard protocol, and things Capital Factory already runs. See `technical-stack.md`.

**The tension worth naming.** A serious reading of this market says buy the inventory layer rather than build it. That reading is probably right, and it is compatible with this recommendation: the store underneath the model may well be someone else's product. What does not exist for purchase, at any price, is the layer that answers the question during planning. The design keeps those two concerns separate specifically so that the first can be replaced without disturbing the second.

---

## Who comes closest, and what each would leave undone

| Product | What it nails | What it would leave undone here |
| --- | --- | --- |
| ServiceNow AI Control Tower | Enterprise-wide governance and portfolio management | Enterprise cost and implementation weight, for an estate of 51 items and eight builders |
| Port | Developer UX, application catalog, skills registry, MCP hub, approvals | Does not replace agent identity, and is a destination people must visit |
| Kore.ai AMP, IBM watsonx Orchestrate | Vendor-neutral agent management across frameworks | Governs agents after they exist; no plan-time hook |
| Airia | Discovery through runtime governance in one AI-specific platform | Same post-hoc shape, plus a new platform boundary around sensitive data |
| JFrog AI Catalog | Governed registries for skills and MCP servers, with versioning and scanning | No portfolio view, no plan-time reuse |
| Okta for AI Agents | Agent identity, ownership and permissions | A different problem, and one this project explicitly does not take on |

No option should be assumed to satisfy every requirement, and none should be dismissed without a real evaluation against a real estate.

---

## The recommended evaluation, if one is run

The earlier version of this document proposed testing internal developer portals. That was too narrow. Two distinct routes should be compared:

- **Developer portal route:** Port, as the strongest single candidate.
- **AI control-plane route:** ServiceNow, Kore.ai, IBM or Airia.

The comparison would answer whether Capital Factory primarily needs a better software operating model, a genuine agent-management platform, or neither at this size. In all three outcomes the plan-time layer is still missing and still has to be built.

---

## Falsifiable criteria

The recommendation changes if any product clears all four. Written down so it can be checked rather than trusted.

1. It reads Capital Factory's existing capability format without a rewrite.
2. It can be queried during planning, from inside the tool the builder is already using, without a person visiting anything.
3. It returns enough to act on, not just a record: contract, invocation, and a usage example.
4. It is affordable and operable by an organization with eight builders and no platform team.

Criterion 2 is the one nothing currently clears. If a product ever clears all four, adopt it, port the content, and spend the time on evaluations and the decision rule instead, which are the parts nobody else will write.

---

## Vendor stability

Two notes for anyone weighing a purchase.

This category has already seen consolidation, and products in it have been acquired and sunset. Any product selected should be evaluated on whether its data can be exported in a form that survives the vendor.

That is also the argument for keeping the capability descriptions in a portable format and treating every layer beneath them as replaceable. The content is the asset. The store is not.

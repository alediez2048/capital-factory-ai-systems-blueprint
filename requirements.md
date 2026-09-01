# Requirements

Status: Proposed baseline requiring Capital Factory validation.
Revised August 29, 2026 following the discovery session and the shift to an intelligence-layer architecture.
Companion documents: `architecture-decision.md`, `system-design.md`, `prd.md`.

## Purpose

These requirements describe what the Capital Factory understanding layer and shared core should accomplish. They do not prescribe vendors. Priorities and acceptance criteria require confirmation during discovery.

Priority definitions:

- **P0** - required to protect access, data, or a critical production workflow.
- **P1** - required for the first production-ready release.
- **P2** - important for scale, efficiency, or long-term maturity.

## What changed in this revision

Three structural changes, recorded rather than applied silently.

**Identity and security moved to an annex.** Systems administration, SSO, and credential management were assigned to a separate IT and security engagement. The IAM and SEC requirements are preserved in full at the end of this document, marked as owned elsewhere, because they remain true and whoever takes that work should not have to rediscover them. They are no longer requirements of this system.

**A new REG and MCP series was added.** The organizational model and the interface that serves it are now the primary product surface, and they had no requirements before.

**CORE-007 and CORE-008 were promoted.** Checking the catalog before building, and identifying extraction candidates, were P2 nice-to-haves. They are now the two mechanisms the whole design exists to deliver, and they are P1.

---

## Governance and ownership

- **GOV-001 (P0):** Every priority application, capability, repository, deployment, and business system must have a named business owner and technical owner.
- **GOV-002 (P1):** Applications and capabilities must have an explicit lifecycle state: experimental, beta, production, paused, deprecated, or retired.
- **GOV-003 (P1):** Shared-core changes must have named reviewers, documented ownership, and an exception process.
- **GOV-004 (P1):** Material architecture decisions must be recorded with context, alternatives, consequences, and approval status.
- **GOV-005 (P1):** Every recommendation the system makes must carry its reasoning. A proposal that cannot be argued with cannot be trusted or corrected.
- **GOV-006 (P1):** No automated process may modify an application without human approval during the first ninety days.

## The organizational model

- **REG-001 (P1):** The model must be generated from source rather than hand-maintained. A model that drifts from reality is worse than no model.
- **REG-002 (P1):** The model must cover five entity types: application, capability, tool, deployment, and data source.
- **REG-003 (P1):** Every capability entry must record what it does, what it never does, inputs, outputs, systems touched, how it is invoked, owner, consumers, and evaluation status.
- **REG-004 (P1):** Capability descriptions must be written to be read by a model, and their quality must be tested. A description too vague to match against is a functional defect, not a documentation gap, because it causes a false negative and a duplicate gets built.
- **REG-005 (P1):** The model must record consumer edges: which application depends on which capability at which version.
- **REG-006 (P1):** Every entry must carry a freshness stamp so that a stale entry is visibly stale.
- **REG-007 (P1):** Entries must be stored in a format that ports if a first-party capability registry ships, so that only the implementation is discarded and never the content.
- **REG-008 (P2):** The model must record live-versus-dormant status, so duplication counts exclude dead code.
- **REG-009 (P1):** The model must be valid and useful when empty, so the architecture can be stood up at an organization that has not started yet.

## The interface

- **MCP-001 (P1):** The understanding layer must be reachable by Claude through MCP, so that no new destination is created and builders continue working in the tools they already use.
- **MCP-002 (P1):** The layer must expose capability search by described behaviour rather than by name. In an estate organized by person, names are the least reliable signal available.
- **MCP-003 (P1):** A search result must return enough to act on: the contract, how to invoke it, and a usage example.
- **MCP-004 (P0):** The layer must serve metadata about capabilities only. It must not proxy, read, or broker data held in systems of record.
- **MCP-005 (P1):** Every query, match, and non-match must be logged with what was ultimately built.
- **MCP-006 (P1):** No builder may be required to know the layer exists in order to benefit from it. Any feature that depends on builder awareness has failed this requirement and must be cut.
- **MCP-007 (P2):** The layer must be able to state the standards a new application in a given context should follow.

## The feedback loop

- **LOOP-001 (P1):** The estate must be scanned on a schedule, read-only, with no ability to modify any application.
- **LOOP-002 (P1):** The scan must detect capabilities implemented more than once, and divergence between copies of the same capability.
- **LOOP-003 (P1):** Every finding must carry exactly one recommendation: extract into the core, patch in place across the copies, or leave alone. Detection without a recommendation transfers the hard judgment back to a human who has less context than the system does.
- **LOOP-004 (P1):** Recommendations must be produced by an explicit, written test rather than by similarity alone. Similarity alone would recommend merging capabilities whose independence is the point.
- **LOOP-005 (P1):** The disposition of every prior proposal must be tracked and visible. An unactioned queue is the known failure mode of this design and must be measurable rather than assumed away.
- **LOOP-006 (P2):** Repeated search non-matches must be surfaced as predicted duplication, ranked by how many separate plans asked for the same missing thing.

## The shared core

- **CORE-001 (P1):** Reusable capabilities must have a canonical source and a documented contract.
- **CORE-002 (P1):** Applications must consume shared capabilities through a controlled, versioned mechanism rather than unmanaged copying.
- **CORE-003 (P1):** Shared capabilities must declare owners, consumers, external systems, permissions, and evaluation requirements.
- **CORE-004 (P1):** Breaking changes must be detectable, communicated to consumers, and recoverable.
- **CORE-005 (P1):** A capability must not enter the core without an owner and an evaluation.
- **CORE-006 (P1):** Deterministic calculation and system I/O should be implemented as testable tools rather than embedded only in model instructions.
- **CORE-007 (P1):** Consequential agent actions must support human approval where policy requires it.
- **CORE-008 (P1):** Applications must be able to remain heavy indefinitely. Coexistence with the core is a permanent supported state, not a migration phase.
- **CORE-009 (P2):** A capability must be removable from the core, returning its logic to consumers, if extraction proves to have been the wrong call.

## AI evaluation and cost

- **AI-001 (P1):** Model calls must be attributable to an application and environment.
- **AI-002 (P1):** Priority workflows must record token usage, latency, failures, and estimated or reported model cost.
- **AI-003 (P1):** Shared capabilities must have repeatable evaluations covering expected behaviour and material safety constraints.
- **AI-004 (P1):** Model or prompt changes must be evaluated before production release for priority capabilities.
- **AI-005 (P1):** Agent loops and tool use must have bounded execution, error handling, and cost controls.
- **AI-006 (P1):** A capability composed of multiple functions must allow each function to be routed to a different model, and the cost and quality difference must be measurable.
- **AI-007 (P2):** The organization should be able to compare model quality, latency, and cost by capability, and re-evaluate when a new model ships.
- **AI-008 (P1):** Scheduled AI workloads must be economically viable on the organization's actual inference arrangement. Continuously running agents are out of scope on subscription inference at this scale.

## Deployment and operations

- **OPS-001 (P1):** Priority production workloads should run on organization-controlled managed infrastructure rather than employee computers.
- **OPS-002 (P1):** Production deployments must be reproducible from a canonical repository.
- **OPS-003 (P1):** Production and non-production environments must be separated appropriately for the application's risk.
- **OPS-004 (P1):** Priority applications must have health checks, error monitoring, deployment history, and accountable alert recipients.
- **OPS-005 (P1):** Priority applications must have a documented and tested rollback or recovery process.
- **OPS-006 (P1):** Scheduled and headless workflows must expose execution status and failures.
- **OPS-007 (P2):** Operational telemetry should use portable standards where practical.

## Documentation and continuity

- **DOC-001 (P1):** Every priority application must document its purpose, owners, source, runtime, dependencies, systems accessed, and operating procedure.
- **DOC-002 (P1):** Every shared capability must document its contract, consumers, permissions, evaluations, and release history.
- **DOC-003 (P1):** The extraction test must exist as a written standard that someone other than its author can apply.
- **DOC-004 (P1):** Deliverables must include runbooks and a named ownership model for after the ninety days.
- **DOC-005 (P2):** Documentation should live alongside the relevant code or configuration and be reviewed with material changes.

---

## Acceptance requirements

These require explicit Capital Factory approval.

1. A verified estate register exists, including how each application reaches the systems it touches.
2. An organizational model is generated from source and is current.
3. A model can correctly answer "do we already do this?" against that model, including the cases where the answer is no.
4. Claude reaches the model through MCP during planning, and reuses an existing capability without the builder being shown anything.
5. A scheduled scan produces reasoned recommendations, and the disposition of prior recommendations is visible.
6. At least one capability has been extracted with an owner, an evaluation, and a demonstrated rollback.
7. An improvement made once to a shared capability demonstrably reaches all of its consumers.
8. Time from plan to first working application has not increased.
9. Runbooks and long-term owners are approved.

The number of systems remediated and capabilities extracted should not be committed before the discovery audit is complete.

Requirement 8 deserves emphasis. If the shared layer makes building slower, the architecture is wrong for this team regardless of how much duplication it removes, and the honest response is to fall back to independent applications with periodic audit.

---

## Annex: identity and security, owned elsewhere

Preserved in full, unchanged in substance, and marked as belonging to a separate IT and security engagement. This system consumes whatever identity model that work produces and is designed to depend on as little of it as possible. These are recorded so that whoever takes the work starts from evidence.

- **IAM-001 (P0):** Production workloads must not depend solely on personal employee identities.
- **IAM-002 (P0):** Privileged human access must use individually assigned accounts protected by MFA.
- **IAM-003 (P0):** Machine access must use scoped organization-controlled service identities where supported.
- **IAM-004 (P0):** The organization must be able to revoke a person without losing access to production systems or breaking priority workloads.
- **IAM-005 (P1):** Access grants, changes, emergency access, and offboarding must have documented procedures and accountable owners.
- **IAM-006 (P1):** Privileged access should be reviewed periodically and follow least-privilege principles.
- **IAM-007 (P0):** Every privileged system must have an organization-controlled administrator and a documented recovery path.

- **SEC-001 (P0):** Production secrets must not be committed to source control.
- **SEC-002 (P0):** Secrets must be stored in an approved secrets system and scoped by application and environment where practical.
- **SEC-003 (P0):** Sensitive data must be classified before enabling prompt, response, or trace-content logging.
- **SEC-004 (P1):** Priority repositories must use branch protections, review controls, dependency scanning, and secret scanning appropriate to their risk.
- **SEC-005 (P1):** Credential rotation must be documented and tested for priority integrations.
- **SEC-006 (P1):** Security-relevant actions and privileged changes must generate auditable records.
- **SEC-007 (P1):** Incident contacts, containment procedures, and recovery responsibilities must be defined.

Two observations that belong with this annex rather than in the body of this document. Applications running on employee machines are both an access issue and a platform coverage gap, and they will surface during the estate mapping. Credentials embedded in application code will surface the same way. Both should be handed over as findings, not remediated by this work.

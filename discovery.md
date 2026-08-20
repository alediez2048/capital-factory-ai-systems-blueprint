# Discovery and Access Audit

Status: Evidence-triaged working draft
Last updated: August 20, 2026
Objective: Collect only the evidence still required to finalize requirements, architecture, technical stack, and a credible 90-day scope.

## Evidence review completed

This question set was checked against the following materials available in the blueprint workspace:

- `Technology Leadership and Operating Mandate - Capital Factory Technology Program`, working draft 0.6, August 6, 2026.
- `CF Read-Ahead - Claude Architecture`, repository survey dated July 22, 2026.
- The current blueprint source register and proposed 90-day program.

The AI Application Inventory, Systems and Admin Ownership Register, and Venture Associate School PRD are referenced by the mandate but were not available for direct inspection in this workspace. Claims attributed to those sources remain `Reported`, not independently verified. The CollabBoard documents are formatting examples and were not used as evidence about Capital Factory.

## Status definitions

- **Answered - reported:** The supplied materials state an answer clearly, but the responsible owner or authoritative system has not yet confirmed it.
- **Partially answered:** The materials provide direction or a subset of the answer; a material decision or operational detail remains open.
- **Unanswered:** The supplied materials do not provide an answer.
- **Verification required:** A documentary answer exists, but repository, administrative-console, configuration, billing, or stakeholder evidence is still required.

This document should guide structured conversations and technical walkthroughs rather than be sent as one large questionnaire. Each confirmed answer should retain an owner, evidence source, date, and status.

## P0 - Mandate and authority

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 1 | Is the shared-skills architecture approved, or should the operator validate it before implementation? | **Partially answered** | The Claude Architecture deck presents a four-layer target architecture and a path for centralizing shared skills. The mandate treats a shared core as an immediate need and calls for its first release. | Confirm whether the deck is an approved constraint or a proposed hypothesis the operator may revise. |
| 2 | What exact decision should this proposal support? | **Answered - reported** | The mandate recommends approving a 90-day program, engaging one full-time on-site operator-engineer at a working contractor budget of $10,000 per month, establishing the shared AI core, stabilizing systems/IAM, and supporting one priority application. | Confirm whether Runpoint is seeking approval to begin the search, approval of the delivery mandate, or both in the next decision meeting. |
| 3 | Who is the executive sponsor, day-to-day decision-maker, and final acceptance owner? | **Partially answered** | Gordon is the executive sponsor; Kyle provides day-to-day direction; Sam leads sourcing and provides technical supervision and architectural review. | The final acceptance owner for the Day-90 production proof is not named. |
| 4 | Which decisions may the operator make independently, and which require stakeholder approval? | **Partially answered** | Gordon sets priorities and approves budget and access. Kyle supplies daily context, decisions, and issue removal. Sam reviews architecture and delivery choices. The operator owns hands-on execution and program integration. | Define approval thresholds for architecture, production changes, security exceptions, spending, data access, and scope changes. |
| 5 | What is explicitly outside the 90-day scope? | **Unanswered** | The mandate defines outcomes but does not provide a formal exclusion list. It does not promise remediation of every system or migration of every application. | Approve explicit exclusions, including any systems, applications, migrations, compliance work, procurement, or support obligations. |
| 6 | Who will own and maintain the resulting platform after Day 90? | **Unanswered** | Employment after the engagement is described as possible, and a long-term staffing recommendation is a Day-90 output. No permanent platform owner is designated. | Name the intended business and technical owners or make owner selection an explicit program decision. |

## P1 - Current application estate

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 7 | Can the operator receive read-only access to the five GitHub repositories and current inventory? | **Unanswered** | A read-only survey of five repositories was completed for the architecture deck, and the mandate says to use the inventory during intake. This does not establish that the future operator has been approved for access. | Confirm access sponsor, repository scope, onboarding method, and timing. |
| 8 | What has changed since the July 22, 2026 architecture survey? | **Unanswered** | No change log or refreshed inventory was supplied. | Re-scan the repositories and reconcile additions, removals, deployments, owners, and material code changes. |
| 9 | Which of the 51 packaged items are active, beta, experimental, paused, duplicated, or retirement candidates? | **Unanswered** | The deck counts 51 items and documents duplicated capabilities. The mandate identifies 31 applications, 16 capabilities, two tools, and one configuration layer, but does not provide lifecycle classifications. | Validate status and business value item by item, starting with priority applications and duplicated capabilities. |
| 10 | Who owns each priority application? | **Unanswered** | No complete business-owner, technical-owner, primary-contact, and backup-contact map was available. | Build the application ownership map from the inventory and stakeholder interviews. |
| 11 | Where does each active application run? | **Partially answered** | The mandate reports hosting split across employee-local and central environments, with Vercel usage and GitHub operating policy incomplete. | Produce an application-by-application deployment map covering provider, project/account, environment, domain, secrets, data stores, and owner. |
| 12 | Which application should be the Day-90 production proof point? | **Partially answered** | The mandate explicitly recommends VA School as the immediate proof point and reports beta testing with an August 10 rollout target. | Confirm its present post-target status, current owner, production risk, acceptance owner, and whether it remains the best proof point. |

## P1 - Shared AI core

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 13 | What is a "skill" technically today? | **Partially answered** | The deck defines a skill conceptually as one reusable, contract-based competency that multiple apps can call; apps own end-to-end jobs, tools are deterministic code, and data/systems are systems of record. | Inspect representative repositories to determine actual packaging: Markdown, code, MCP tools, scripts, services, or combinations. |
| 14 | How do applications discover, load, invoke, and update existing skills? | **Unanswered** | The deck describes the intended shared-repository model and downward call flow, not the current runtime and distribution mechanics. | Document current and proposed discovery, versioning, dependency, invocation, release, and upgrade paths. |
| 15 | Who created the Claude Architecture deck and conducted the repository survey? | **Partially answered** | The PowerPoint metadata identifies Gordon Daugherty as the last person to modify the deck. That does not establish authorship or who performed the survey. | Confirm the author, surveyor, methodology, repository access used, and accountable technical reviewer. |
| 16 | Which duplicated capability is the safest and most valuable first extraction candidate? | **Partially answered** | The deck identifies voice drafting, safe database writes, meeting scheduling, inbox triage, AngelList compliance, principal simulation, stale-fact linting, and the autonomy ladder as duplicated. Safe database writes has a documented missing guardrail, making it a strong risk-led candidate; voice drafting is also used broadly. | Score candidates by risk reduction, consumer count, coupling, testability, migration effort, and business value; obtain owner approval. |
| 17 | How do the marketplace, `CODEOWNERS`, `MERGE-GATE`, `skill-lint`, and evaluation suite work today? | **Partially answered** | The deck reports that all five mechanisms exist in production and proposes reusing them for the shared core. | Inspect configurations, enforcement points, owners, coverage, failure behavior, and evidence that controls cannot be bypassed. |
| 18 | Is Claude required, preferred, or one supported provider? | **Unanswered** | The source deck is Claude-specific and proposes rerunning evaluations on new Claude model releases. It does not state a contractual or architectural requirement to remain Claude-only. | Confirm provider policy, portability expectations, approved models, data terms, and whether model routing is in scope. |

## P1 - Identity, security, and data

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 19 | Who administers the critical systems and accounts? | **Partially answered** | The mandate reports 48 systems; current owner or super-admin responsibility for 21 sits with Josh and nine with Fred. Twenty-four are designated for temporary transfer to Kyle, only four are marked complete, and eight lack a long-term owner. | Inspect the ownership register and confirm the administrator, recovery owner, long-term owner, backup, and transfer state for every priority system. |
| 20 | Which systems depend on personal identities, shared credentials, or former employees? | **Partially answered** | The mandate explicitly identifies critical-system dependence on individual identities and unclear ownership as an immediate risk. It does not enumerate shared credentials or former-employee dependencies. | Verify identity type, recovery path, credential custody, employment status, and break-glass access system by system. |
| 21 | What sensitive data do priority applications access or transmit to AI providers? | **Partially answered** | The mandate reports that several critical SaaS systems contain sensitive data or control important work. No application-level data-flow or AI-provider disclosure inventory was supplied. | Map data classes, systems of record, model/provider destinations, retention, subprocessors, and approval requirements for the reference application. |
| 22 | May prompts, responses, tool inputs, and traces be stored externally? | **Unanswered** | No observability data policy was supplied. | Obtain a decision from the security/data owner by data classification and environment. |
| 23 | What SSO, MFA, service-account, access-review, and offboarding standards exist? | **Unanswered** | The mandate calls for practical IAM standards to be put into use, which indicates the desired work but does not document the current standard. | Collect current policies and configurations, then identify gaps against the approved operating standard. |
| 24 | What security, contractual, regulatory, or retention restrictions apply? | **Unanswered** | No authoritative policy, contract inventory, regulatory classification, or retention schedule was supplied. | Identify policy and legal owners and obtain applicable requirements before final architecture approval. |

## P1 - Production and acceptance

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 25 | What must demonstrably work by Day 90? | **Answered - reported** | The mandate defines three proofs: a practical IAM model in use, the first shared AI core release working, and one priority workflow operating on managed infrastructure. It also calls for closing the first systems-control risk tranche and documenting the future application path. | Convert these outcomes into measurable acceptance tests, named owners, and evidence requirements. |
| 26 | What availability, recovery, rollback, audit, and human-approval requirements apply? | **Partially answered** | The materials emphasize resilient access, recovery, human approval for core-maintenance automation, and managed infrastructure, but contain no measurable service levels or recovery objectives. | Define SLOs, RTO/RPO, rollback evidence, audit-event scope, incident handling, and human approval gates for the reference application. |
| 27 | Who executes and signs off on acceptance testing? | **Unanswered** | Gordon and Kyle retain hands-on authority and Sam provides technical oversight, but no acceptance-test executor or final signatory is named. | Name business, technical, security, and final acceptance roles. |
| 28 | What production disruption is acceptable during migration? | **Unanswered** | No maintenance-window, downtime, freeze, rollback-trigger, or user-communication tolerance was supplied. | Obtain application-owner approval before migration planning. |

## P2 - Budget and capacity

| ID | Question | Current status | Evidence-based answer | Remaining confirmation |
| --- | --- | --- | --- | --- |
| 29 | Does the stated $10,000 monthly budget cover only operator labor? | **Answered - reported** | The mandate calls this the working contractor budget for one full-time operator-engineer. | Confirm taxes/fees, equipment, travel, and whether any delivery expenses are included contractually. |
| 30 | What additional platform budget is available? | **Unanswered** | No separate cloud, security, observability, model/API, tooling, or contingency budget is stated. | Approve a non-labor budget or explicit spending thresholds. |
| 31 | Which internal engineers or volunteers are available? | **Partially answered** | The mandate allows volunteer engineers working 8-10 hours per week on bounded projects under the operator's program ownership. | Confirm names, skills, number of contributors, start dates, availability, conflicts, and assignment authority. |
| 32 | What SaaS, cloud, and AI usage data can support cost modeling? | **Answered - reported as pending** | Financial SaaS data remains outstanding. The requested fields are spend, seats, contract terms, renewal dates, and usage. No complete cloud or AI token/cost dataset is identified. | Obtain the financial SaaS dataset plus cloud invoices, AI-provider usage exports, account/project mappings, and cost owners. |

## Questions that can be removed from the initial stakeholder interview

The following no longer need to be asked as open-ended questions; they should be presented for confirmation:

1. The proposed program combines operator engagement, systems/IAM stabilization, a shared-core release, and one managed priority workflow.
2. Gordon is executive sponsor, Kyle provides day-to-day direction, and Sam provides technical oversight.
3. VA School was the recommended immediate proof point in the August 6 mandate.
4. The working contractor budget is $10,000 per month.
5. Volunteers may take bounded projects at approximately 8-10 hours per week while the operator retains integration ownership.
6. Day-90 proof consists of a practical IAM model, a working shared-core release, and one priority workflow on managed infrastructure.
7. Financial SaaS data was still pending when the mandate was written.

## Highest-value unresolved questions for the first CF session

To avoid overwhelming stakeholders, the first working session should focus on these eight decisions:

1. Is the Claude target architecture approved, or may the operator revise it after technical validation?
2. Does VA School remain the Day-90 proof point, and who accepts it?
3. What changed in the application estate after July 22, 2026?
4. Who owns the platform after Day 90?
5. Which production, security, and data-policy constraints govern the reference application?
6. Which repository, hosting, identity, and billing access can be approved for the initial audit?
7. What non-labor budget and spending authority are available?
8. What is explicitly outside the 90-day mandate?

## Initial read-only access requested

Access should be time-bounded, least-privilege, individually assigned, and protected with MFA. Secret values and unrestricted production data are not required for the initial audit.

| System | Initial access | Purpose | Approval owner | Status |
| --- | --- | --- | --- | --- |
| GitHub | Organization/repository read | Verify repositories, dependencies, workflows, owners, and controls | To confirm | Not requested |
| Vercel and other hosting | Project viewer | Map deployments, domains, environments, and runtime ownership | To confirm | Not requested |
| 1Password | Guided review or scoped metadata | Understand vault and machine-identity model without exporting secrets | To confirm | Not requested |
| Google Workspace | Admin-led configuration review | Understand identity, MFA, recovery, and offboarding | To confirm | Not requested |
| AI provider accounts | Usage/billing viewer | Map providers, keys, models, tokens, and costs | To confirm | Not requested |
| Monitoring platforms | Viewer | Assess errors, uptime, traces, and alert ownership | To confirm | Not requested |
| Asana or delivery system | Scoped viewer | Understand active backlog, incidents, and ownership | To confirm | Not requested |

## Minimum audit outputs

The initial audit should produce:

1. A verified application and system inventory for priority assets.
2. A stakeholder and ownership map.
3. An access and administrative-risk summary.
4. A current-state architecture and deployment map.
5. A prioritized list of shared-capability extraction candidates.
6. A validated reference application and Day-90 acceptance criteria.
7. Decisions or decision owners for unresolved stack choices.

# Production Requirements

Status: Proposed baseline requiring Capital Factory validation

## Purpose

These requirements describe what the Capital Factory AI operating system and shared core should accomplish. They do not prescribe final vendors. Priorities and acceptance criteria must be confirmed during discovery.

Priority definitions:

- **P0** - Required to protect access, data, or a critical production workflow.
- **P1** - Required for the first production-ready shared-core release.
- **P2** - Important for scale, efficiency, or long-term maturity.

## Governance and ownership

- **GOV-001 (P0):** Every priority application, capability, repository, deployment, and business system must have a named business owner and technical owner.
- **GOV-002 (P0):** Every privileged system must have an organization-controlled administrator and documented recovery path.
- **GOV-003 (P1):** Applications and capabilities must have an explicit lifecycle state: experimental, beta, production, paused, deprecated, or retired.
- **GOV-004 (P1):** Shared-core changes must have named reviewers, documented ownership, and an exception process.
- **GOV-005 (P1):** Material architecture decisions must be recorded with context, alternatives, consequences, and approval status.

## Identity and access

- **IAM-001 (P0):** Production workloads must not depend solely on personal employee identities.
- **IAM-002 (P0):** Privileged human access must use individually assigned accounts protected by MFA.
- **IAM-003 (P0):** Machine access must use scoped organization-controlled service identities where supported.
- **IAM-004 (P0):** Capital Factory must be able to revoke a user without losing access to production systems or breaking priority workloads.
- **IAM-005 (P1):** Access grants, changes, emergency access, and offboarding must have documented procedures and accountable owners.
- **IAM-006 (P1):** Privileged access should be reviewed periodically and follow least-privilege principles.

## Security and secrets

- **SEC-001 (P0):** Production secrets must not be committed to source control.
- **SEC-002 (P0):** Secrets must be stored in an approved secrets system and scoped by application and environment where practical.
- **SEC-003 (P0):** Sensitive data must be classified before enabling prompt, response, or trace-content logging.
- **SEC-004 (P1):** Priority repositories must use branch protections, review controls, dependency scanning, and secret scanning appropriate to their risk.
- **SEC-005 (P1):** Credential rotation must be documented and tested for priority integrations.
- **SEC-006 (P1):** Security-relevant actions and privileged changes must generate auditable records.
- **SEC-007 (P1):** The program must define incident contacts, containment procedures, and recovery responsibilities.

## Shared AI core

- **CORE-001 (P1):** Reusable capabilities must have a canonical source and a documented contract.
- **CORE-002 (P1):** Applications must consume shared capabilities through a controlled, versioned mechanism rather than unmanaged copying.
- **CORE-003 (P1):** Shared capabilities must declare their owners, consumers, external systems, permissions, and evaluation requirements.
- **CORE-004 (P1):** Breaking changes must be detectable, communicated to consumers, and recoverable.
- **CORE-005 (P1):** Deterministic calculations and system I/O should be implemented as testable tools rather than embedded only in model instructions.
- **CORE-006 (P1):** Sensitive or consequential agent actions must support human approval where required by policy.
- **CORE-007 (P2):** New application planning should check the capability catalog before creating duplicate functionality.
- **CORE-008 (P2):** The platform should identify extraction candidates when reusable logic appears inside applications.

## AI evaluation and cost

- **AI-001 (P1):** Model calls must be attributable to an application and environment.
- **AI-002 (P1):** Priority workflows must record token usage, latency, failures, and estimated or reported model cost.
- **AI-003 (P1):** Shared capabilities must have repeatable evaluations covering expected behavior and material safety constraints.
- **AI-004 (P1):** Model or prompt changes must be evaluated before production release for priority capabilities.
- **AI-005 (P1):** Agent loops and tool use must have bounded execution, error handling, and cost controls.
- **AI-006 (P2):** The architecture should avoid unnecessary provider lock-in unless Capital Factory explicitly approves a provider-specific strategy.
- **AI-007 (P2):** The organization should be able to compare model quality, latency, and cost by capability.

## Deployment and operations

- **OPS-001 (P0):** Priority production workloads must run on organization-controlled managed infrastructure rather than employee computers.
- **OPS-002 (P1):** Production deployments must be reproducible from a canonical repository.
- **OPS-003 (P1):** Production and non-production environments must be separated appropriately for the application's risk.
- **OPS-004 (P1):** Priority applications must have health checks, error monitoring, deployment history, and accountable alert recipients.
- **OPS-005 (P1):** Priority applications must have a documented and tested rollback or recovery process.
- **OPS-006 (P1):** Scheduled and headless workflows must expose execution status and failures.
- **OPS-007 (P1):** Production changes must follow an approved review and release process.
- **OPS-008 (P2):** Operational telemetry should use portable standards where practical.

## Documentation and continuity

- **DOC-001 (P1):** Every priority application must document its purpose, owners, source, runtime, dependencies, systems accessed, and operating procedure.
- **DOC-002 (P1):** Every shared capability must document its contract, consumers, permissions, evaluations, and release history.
- **DOC-003 (P1):** Day-90 deliverables must include runbooks and a post-engagement ownership model.
- **DOC-004 (P2):** Documentation should be maintained alongside the relevant code or configuration and reviewed with material changes.

## Proposed Day-90 acceptance requirements

These require explicit Capital Factory approval:

1. A verified priority estate and ownership register is in use.
2. The first critical IAM and administrative-risk tranche is closed.
3. A canonical shared-core repository is operational.
4. At least one approved reusable capability is consumed through the shared-core model.
5. One priority application runs on managed infrastructure with reproducible deployment.
6. That application exposes health, errors, AI usage, cost, and evaluation evidence appropriate to its data classification.
7. Recovery or rollback has been demonstrated.
8. Long-term owners and the next-quarter roadmap are approved.

The number of systems remediated and capabilities extracted should not be committed until the discovery audit is complete.


# Proposed 90-Day Delivery Program

Status: Working scope requiring Capital Factory validation

## Outcome

Establish institutional control of the priority technology estate, implement the first working release of the shared AI core, and prove the operating model through one priority application on managed infrastructure.

## Operating model

- One full-time operator-engineer owns day-to-day execution.
- Gordon serves as executive sponsor.
- Kyle provides day-to-day direction, context, and issue removal.
- Sam provides technical supervision and architectural review.
- Capital Factory retains authority over budget, access, security policy, priority, and acceptance.
- Volunteers or adjunct engineers receive bounded tasks with clear acceptance criteria; the operator retains program integration responsibility.

This role structure is reported from the working mandate and requires confirmation.

## Phase 1 - Verify and protect

Indicative period: Days 1-20

### Objectives

- Confirm the mandate, decision authority, and Day-90 proof point.
- Verify the application and systems inventories.
- Obtain least-privilege read-only access.
- Assess VA School or the selected priority application's current condition.
- Identify critical identity, ownership, secrets, and hosting risks.
- Validate the shared-core architecture and select the first capability extraction.
- Run the distribution spike: package one existing skill through the Claude private plugin marketplace and test version pinning, rollback, and telemetry (feeds ADR-001 and ADR-002; see technical-stack.md).

### Outputs

- Verified priority technology estate register.
- Stakeholder, owner, administrator, and POC map.
- Current-state application, deployment, identity, and data-flow map.
- Immediate-risk backlog.
- Distribution spike result with a recommendation for ADR-001.
- Approved architecture decisions required for Phase 2.
- Baseline acceptance criteria.

## Phase 2 - Stabilize and implement

Indicative period: Days 21-65

### Objectives

- Close the first critical IAM and systems-control risks.
- Establish the canonical shared-core repository and contribution controls.
- Implement the selected capability contract, tests, and evaluations.
- Move the reference workload or its risky local dependencies onto managed infrastructure.
- Add appropriate secrets, service identities, deployment controls, health, errors, and AI usage attribution.
- Establish one coordinated delivery backlog.

### Outputs

- First working shared-core release.
- At least one governed shared capability.
- Reference application integration.
- Managed deployment and rollback path.
- Initial operational dashboard or catalog view.
- Runbooks for the implemented production path.

The number of applications and capabilities included will be determined after Phase 1.

## Phase 3 - Prove and transfer

Indicative period: Days 66-90

### Objectives

- Run acceptance, recovery, and regression scenarios.
- Demonstrate application health, errors, deployment evidence, AI usage, cost, and evaluation results.
- Document the path for future employee-built applications.
- Assign long-term owners and operating responsibilities.
- Deliver the next-quarter roadmap and staffing recommendation.

### Outputs

- Accepted production proof point.
- Demonstrated rollback or recovery.
- Production-readiness scorecard and remaining-risk register.
- Shared-core contribution, versioning, evaluation, and release process.
- Post-90-day ownership model.
- Prioritized roadmap and cost model.

## Proposed Day-90 proof

By Day 90, Capital Factory should be able to demonstrate that:

1. Priority technology assets have accountable owners and recoverable institutional access.
2. A canonical shared AI core is operating under documented review and release controls.
3. At least one reusable capability has been safely centralized and adopted.
4. One priority workflow runs on organization-controlled managed infrastructure.
5. Its deployment, health, errors, AI usage, cost, and evaluations are visible at an appropriate level for its data classification.
6. A rollback or recovery path has been tested.
7. Future applications have a documented intake and production-readiness path.
8. Permanent ownership and the next implementation tranche are approved.

## Scope controls

The engagement should not promise complete migration of all 51 packaged items or remediation of all 48 systems before discovery. Scope changes should be evaluated against:

- Business criticality.
- Security and continuity risk.
- Dependency complexity.
- Availability of owners and access.
- Test and evaluation coverage.
- Impact on the reference application's delivery.
- Ability to leave a maintainable system after Day 90.

## Immediate decisions requested from Capital Factory

1. Confirm the purpose and authority of the engagement.
2. Confirm whether the Claude shared-skills architecture is approved or subject to validation.
3. Identify the reference application and acceptance owner.
4. Approve read-only discovery access and named technical contacts.
5. Confirm security and data-policy owners.
6. Confirm the non-labor platform budget.
7. Confirm the expected post-90-day ownership model.


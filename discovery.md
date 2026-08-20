# Discovery and Access Audit

Status: Proposed initial audit  
Objective: Collect the minimum evidence required to finalize requirements, architecture, technical stack, and a credible 90-day scope.

## How to use this document

This is not intended to be sent as a large questionnaire. It should guide a small number of structured conversations and technical walkthroughs. Each answer should include an owner, evidence source, and status.

Suggested answer states: `Unknown`, `Reported`, `Partially verified`, `Verified`, `Approved`, or `Blocked`.

## P0 - Mandate and authority

These questions should be answered before finalizing the proposal.

1. Is the shared-skills architecture approved, or should the operator validate it before implementation?
2. What exact decision should this proposal support: selecting an operator, approving a 90-day program, approving the shared core, or a combination?
3. Who is the executive sponsor, day-to-day decision-maker, and final acceptance owner?
4. Which decisions may the operator make independently, and which require approval from Gordon, Kyle, Sam, or another stakeholder?
5. What is explicitly outside the 90-day scope?
6. Who will own and maintain the resulting platform after Day 90?

## P1 - Current application estate

These answers are required before detailed architecture and migration decisions.

7. Can the operator receive read-only access to the five GitHub repositories and the current application inventory?
8. What has changed since the July 22, 2026 architecture survey?
9. Which of the 51 packaged items are actively used, in beta, experimental, paused, duplicated, or candidates for retirement?
10. Who is the business owner, technical owner, primary point of contact, and backup contact for each priority application?
11. Where does each active application run: Vercel, another cloud provider, a scheduled platform, or an employee machine?
12. Which application should be the Day-90 production proof point? Is VA School still the preferred candidate, and what is its current launch status?

## P1 - Shared AI core

13. What is a "skill" technically today: Markdown instructions, code, package, MCP tool, runtime service, or a combination?
14. How do applications discover, load, invoke, and update existing skills?
15. Who created the Claude Architecture deck and conducted the repository survey?
16. Which duplicated capability is the safest and most valuable first extraction candidate?
17. How do the plugin marketplace, `CODEOWNERS`, `MERGE-GATE`, `skill-lint`, and the existing evaluation suite work today?
18. Is Claude a required platform, a current preference, or one provider that the architecture should support?

## P1 - Identity, security, and data

19. Who administers GitHub, Vercel, Google Workspace, 1Password, domains and DNS, cloud accounts, and AI-provider accounts?
20. Which systems still depend on personal identities, shared credentials, or former employees?
21. What sensitive data do the priority applications access or transmit to AI providers?
22. May prompts, model responses, tool inputs, and traces be stored in external observability platforms?
23. What identity provider, SSO, MFA, service-account, access-review, and offboarding standards already exist?
24. What security, contractual, regulatory, or data-retention restrictions must the architecture observe?

## P1 - Production and acceptance

25. What must demonstrably work by Day 90?
26. What availability, recovery, rollback, audit, and human-approval requirements apply to the reference application?
27. Who will execute and sign off on acceptance testing?
28. What level of production disruption is acceptable during migration?

## P2 - Budget and capacity

29. Does the stated $10,000 monthly budget cover only operator labor?
30. What additional budget is available for cloud infrastructure, security, observability, and platform services?
31. Which internal engineers or volunteers will be available, for how many hours, and under whose direction?
32. What current SaaS, cloud, and AI usage data can be provided for cost modeling?

## Initial read-only access requested

Access should be time-bounded, least-privilege, individually assigned, and protected with MFA. Secret values and unrestricted production data are not required for the initial audit.

| System | Initial access | Purpose | Approval owner | Status |
| --- | --- | --- | --- | --- |
| GitHub | Organization/repository read | Verify repositories, dependencies, workflows, owners, and controls | Unknown | Not requested |
| Vercel and other hosting | Project viewer | Map deployments, domains, environments, and runtime ownership | Unknown | Not requested |
| 1Password | Guided review or scoped metadata | Understand vault and machine-identity model without exporting secrets | Unknown | Not requested |
| Google Workspace | Admin-led configuration review | Understand identity, MFA, recovery, and offboarding | Unknown | Not requested |
| AI provider accounts | Usage/billing viewer | Map providers, keys, models, tokens, and costs | Unknown | Not requested |
| Monitoring platforms | Viewer | Assess errors, uptime, traces, and alert ownership | Unknown | Not requested |
| Asana or delivery system | Scoped viewer | Understand active backlog, incidents, and ownership | Unknown | Not requested |

## Minimum audit outputs

The initial audit should produce:

1. A verified application and system inventory for priority assets.
2. A stakeholder and ownership map.
3. An access and administrative-risk summary.
4. A current-state architecture and deployment map.
5. A prioritized list of shared-capability extraction candidates.
6. A validated reference application and Day-90 acceptance criteria.
7. Decisions or decision owners for unresolved stack choices.


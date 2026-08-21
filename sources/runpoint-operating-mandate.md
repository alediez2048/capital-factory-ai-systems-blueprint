# Technology Leadership and Operating Mandate - Capital Factory Technology Program

Source status: Text reconstruction of the Runpoint proposal, working draft 0.6, dated August 6, 2026. Transcribed from the document as shared; not the original file. Evidence status: Reported.

Prepared for: Capital Factory
Prepared by: Runpoint
Date: August 6, 2026
Status: Working draft 0.6

Recommendation: Engage one full-time, on-site operator-engineer for 90 days at a working budget of $10,000 per month. The operator should secure the systems and IAM foundation, establish a shared AI application core, and support one priority application through delivery.

## 01 - Executive summary

The priority is to turn AI strategy into execution. The points below are ordered by importance to the 90-day program.

1. Build the shared AI application core. The current inventory identifies 51 packaged items across five repositories, including 31 applications and about 10 capabilities implemented more than once. A centrally maintained core is now an immediate operating need.
2. Add a dedicated operator-engineer. A full-time, on-site operator-engineer should own hands-on execution across the systems and AI architecture work for 90 days. Employment afterward remains possible if the engagement succeeds.
3. Secure access and ownership. The new register lists 48 systems. Twenty-four are designated for temporary transfer to Kyle, but only four are marked complete. Eight lack a defined long-term owner.
4. Use VA School as an immediate proof point. VA School is already in beta-stage testing with an August 10 rollout target. The operator should protect that launch, then use its architecture and operating needs to test the shared-core model.
5. Keep Capital Factory hands-on. Gordon and Kyle set goals, priorities, and tradeoffs. Kyle provides day-to-day direction. Sam leads the search and provides technical supervision and architectural guardrails.

## 02 - Current situation

Four conditions define the mandate. The program must connect the strategic ambition to the operating work.

Critical SaaS foundation: AngelList and Asana remain business-critical. Airtable, Google Workspace, 1Password, GitHub, Vercel, and Notion also contain sensitive data or control important work.

Known control gaps: The register places current owner or super-admin responsibility for 21 systems with Josh and nine with Fred. Application hosting is split across local and central environments, while Vercel usage and the GitHub estate lack a complete operating policy.

Large application estate: The current inventory identifies 31 applications, 16 reusable capabilities, two tools, and one configuration layer. Distribution and documentation remain inconsistent across five repositories.

Shared core is the leverage point: About 10 capabilities have been implemented two to four times, and only one item has a test or evaluation suite. A maintained shared layer would reduce drift and give new applications a safer foundation.

## 03 - Diagnosis

Control comes first. The issues below should set the initial work order.

| Finding | Consequence | Priority |
| --- | --- | --- |
| No dedicated owner for technology execution | The AI strategy remains distributed across people and experiments | Immediate |
| Critical systems depend on individual identities | Loss of access, weak recovery, and unclear accountability | Immediate |
| Hosting and deployment paths are unclear | Local applications, Vercel projects, and GitHub repositories cannot yet be managed as one estate | Immediate |
| No shared AI application core | Duplicated capabilities have already drifted, including a compliance guardrail missing a required safety gate | Immediate |
| VA School is approaching launch | The operator must protect near-term delivery while improving the architecture beneath future applications | High |
| Financial SaaS data has not been provided | Spend, seat usage, renewals, and savings opportunities cannot yet be assessed | Medium |

## 04 - 90-day mandate

Move through three phases within 90 days. The phases are sequential and timing remains flexible. The team should advance as quickly as conditions allow.

Proposed mandate: Run two coordinated workstreams within 90 days: put a resilient IAM and systems-control model into use, and establish the first working release of a centrally maintained AI application core.

Phase 1 - Establish the baseline

- Review the VA School PRD, AI application inventory, and systems and admin ownership register
- Protect the August 10 VA School rollout and capture its architectural needs
- Validate the ownership register and complete priority transfers. Assign long-term owners where none are defined
- Document current Vercel, GitHub, and application-hosting patterns
- Add the Portfolio Intelligence Agent PRD and financial SaaS data as they arrive

Phase 2 - Stabilize and build

- Put practical IAM standards into use for the highest-risk systems
- Define the shared skills, system connections, model selection, and deployment layer
- Move priority workloads off employee machines
- Classify existing AI applications by value and production need
- Establish one coordinated delivery backlog

Phase 3 - Prove and decide

- Close the first systems-control risk tranche
- Put the first shared-core release into use
- Support one priority workflow on managed infrastructure
- Document the path for future employee-built applications
- Deliver the roadmap and long-term staffing recommendation

## 05 - Role scorecard: Operator-Engineer

This is a full-time, on-site 90-day delivery role below the executive level.

Engagement: The operator-engineer translates Capital Factory's priorities into hands-on work across systems, IAM, deployment, and AI applications. The working contractor budget is $10,000 per month, and Capital Factory is ready to start now. Conversion to employment remains possible after the initial 90 days.

Gordon is the executive sponsor. Kyle provides day-to-day direction. Sam leads the search and provides technical supervision and architectural review as a pro bono Runpoint advisor.

| Area | Weight | Evidence |
| --- | --- | --- |
| Systems and IAM judgment | 25% | Stabilized a changing environment with practical identity, recovery, and access controls |
| Hands-on delivery | 20% | Can inspect, configure, and build with Codex and Claude Code |
| AI application architecture | 20% | Designed shared capabilities that support multiple applications without locking the organization into one model or pattern |
| Deployment, code, and data | 15% | Owned production workflows across repositories, hosting environments, and business systems |
| Stakeholder communication | 10% | Can write and explain a concise decision brief |
| Focus and ownership | 10% | Can work full time and own an outcome end to end |

## 06 - Search and management

Use a small, evidence-based funnel. Sam leads the search and technical screen. Capital Factory chooses among a small finalist slate and retains hiring authority.

1. Continue asynchronous intake. Use the VA School PRD, AI application inventory, and systems and admin ownership register now. Add the Portfolio Intelligence Agent PRD and financial SaaS data when they are available.
2. Open the operator search. Start with Sam's Austin consultant community and a small parallel referral channel. Search against the full-time, on-site requirement and $10,000 monthly working budget.
3. Screen for evidence. Review relevant work, availability, conflicts, rate, and the same 90-day working exercise.
4. Present two or three. Capital Factory meets only candidates who clear every core scorecard category.
5. Select and contract. Capital Factory contracts the operator against written 90-day outcomes. Employment afterward remains an option based on results.
6. Manage by phase. Gordon and Kyle stay hands-on with goals, priorities, and tradeoffs. Sam reviews technical direction and long-term adaptability.

Roles: Gordon, executive sponsor - sets priorities, approves budget and access, and makes the final candidate decision. Kyle, day to day - provides daily context, access, decisions, and issue removal for the operator-engineer. Sam, pro bono advisor - leads sourcing and technical screening, reviews architecture and delivery choices, and helps the team avoid decisions that limit future change.

Adjunct resources: Volunteer engineers working 8 to 10 hours per week can take bounded projects that emerge from the coordinated backlog. Each project should have a named owner, a clear deliverable, and an acceptance standard. The full-time operator-engineer retains program ownership.

## 07 - Launch inputs

Use the available inputs. The available materials are enough to refine the mandate and begin outreach.

Available now: Venture Associate School PRD (draft 1.0, 90-95% complete, beta-stage testing underway); AI application inventory (read-only audit of five repositories and the current skills and applications estate); systems and admin ownership register (48 systems with current and long-term owner fields, transfer targets, and keep or retire decisions).

Working budget: Use $10,000 per month as the working contractor budget.

Pending inputs: The Portfolio Intelligence Agent PRD is expected in about two weeks. Financial SaaS data remains outstanding: spend, seats, contract terms, renewal dates, and usage.

Day-90 proof: A practical IAM model is in use, the first shared AI core release is working, and one priority workflow runs on managed infrastructure.

Next move: Runpoint finalizes the candidate-safe mandate and opens the search while the remaining inputs arrive.

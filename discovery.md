# Discovery and Access Audit

Status: Revised after the Gordon Daugherty call of August 26, 2026.
Objective: collect only the evidence still required. Questions the call answered have been moved out of the interview and into the record.

Evidence base: the operating mandate (working draft 0.6, August 6, 2026), the Claude Architecture read-ahead (repository survey, July 22, 2026), and `call-notes-gordon.md`, which is now the primary source. Where the mandate and the call conflict, the call wins and the conflict is named.

## Status definitions

- **Answered - verified on the call:** the sponsor stated it directly.
- **Answered - reported:** the supplied materials state it, but no owner has confirmed it.
- **Partially answered:** direction exists, a material decision remains open.
- **Unanswered.**
- **Out of scope:** assigned elsewhere by the sponsor. Recorded, not pursued.

---

## Answered on the call, removed from the interview

Present these for confirmation rather than asking them.

| Was | Now |
| --- | --- |
| Q15, who authored the architecture deck | Gordon wrote it himself, by prompting Claude to survey the repositories. "That's my presentation, by the way. That's the work that I did that myself." |
| Q1, is the architecture approved or open to revision | Open, emphatically. "Ignore the approach that I was thinking we would take unless you conclude it is a viable approach." Answered in `architecture-decision.md`. |
| Q2, what decision does this support | Engagement of one operator for an architecture-and-core project, priced by milestone, with the app layer as a separate later project. |
| Q5, what is outside the 90-day scope | Identity and systems administration, the app layer, and any application migration. See the exclusion list in `90-day-proposal.md`. |
| Q29, does $10,000 per month cover only labour | Obsolete. Retainer pricing is replaced by milestones. |
| Q12, is VA School the proof point | Never mentioned by the sponsor in fifty-six minutes. The premise came from the mandate alone and has been withdrawn. |
| Q25, what must work by Day 90 | Restated as five milestone acceptance tests, each independently checkable. |
| Q31, volunteers | Not raised. Hours are the operator's to manage: "One week you work twenty or twenty five hours, another week you work thirty five. It's up to you." |

---

## P0 - The one question everything depends on

| ID | Question | Status | What we know | What is needed |
| --- | --- | --- | --- | --- |
| **14** | **How do the fifty-one items actually reach the systems they touch?** | **Unanswered, and the sponsor said so** | "In many cases it's MCPs, some cases it's maybe an API or something custom built. I don't know." | A per-application map: MCP server, direct API, custom integration, or human in the loop. Every extraction estimate in every other document is an informed guess until this exists. This is Milestone 1. |

If the estate reaches its systems through a handful of shared MCP servers, extraction is mostly packaging. If it reaches them through thirty-one hand-rolled integrations, this is a different project, and the honest thing is to say so in week three rather than month three.

---

## P0 - Mandate and authority

| ID | Question | Status | What we know | What is needed |
| --- | --- | --- | --- | --- |
| 3 | Who is the executive sponsor and final acceptance owner? | **Partially answered** | Gordon is sponsor and, for now, acting architect: "I seem to be the only one on the team that is thinking about our architecture." Sam advises on the hiring decision. | Whether Gordon accepts each milestone himself or delegates. |
| 4 | Which decisions may the operator make independently? | **Partially answered** | Architecture judgment is explicitly delegated. Nothing may change an application without approval. | Thresholds for spend, data access, and scope change. |
| 6 | Who owns the platform after the engagement? | **Unanswered** | Not raised on the call. The console in `prd.md` has exactly one user and that user is unnamed. | Name the intended owner, or make owner selection an explicit milestone-5 output. |
| 34 | What is Kyle's role in this engagement? | **Unanswered** | The mandate names him for day-to-day direction. He was not mentioned once on the call. | Confirm whether he is involved at all. |

---

## P1 - The estate

| ID | Question | Status | What we know | What is needed |
| --- | --- | --- | --- | --- |
| 7 | Read-only access to the five repositories | **Unanswered** | The survey was done by Gordon with Claude. That does not establish operator access. | Access sponsor, repository scope, onboarding method, timing. This gates Milestone 1. |
| 8 | What changed since July 22, 2026? | **Unanswered** | No change log supplied, and the team has kept shipping throughout. | Re-scan and reconcile. |
| 9 | Which of the fifty-one are live versus dormant? | **Unanswered** | No lifecycle classification exists. | Live-versus-dormant status per item. A duplication count that includes dead code overstates the problem and misdirects the first extractions. |
| 10 | Who owns each application? | **Unanswered** | Three builders named across the materials and the call: Gordon, Jamie, Nick. Drew and Caroline appear in the survey. | Business owner, technical owner, and backup per priority application. |
| 11 | Where does each application run? | **Partially answered** | Hosting is split between employee-local and central, with Vercel in use. | Provider, environment, secrets location, and owner per application. Laptop runtimes are a coverage gap the console must surface. |
| 35 | Which capabilities have an evaluation? | **Answered - reported** | One evaluation suite across fifty-one items. | Confirm. This single number caps how fast the core can safely grow and is the binding constraint on Milestone 5. |

---

## P1 - The core

| ID | Question | Status | What we know | What is needed |
| --- | --- | --- | --- | --- |
| 13 | What is a capability technically today? | **Partially answered** | Conceptually defined in the deck. Packaging unverified. | Inspect representative repositories: Markdown, code, MCP tools, scripts, services, or combinations. |
| 16 | Which duplicated capability is the first extraction? | **Deliberately deferred** | Safe database writes has a documented missing guardrail and is the strongest risk-led candidate. Voice drafting is the most widely duplicated but likely fails the propagation criterion, since Drew does not want Jamie's improvements. | This is an output of the first sweep, not a decision to make in advance. Answering it early would be guessing. |
| 17 | How do the marketplace, CODEOWNERS, MERGE-GATE, skill-lint, and the eval suite work today? | **Partially answered** | All five reported in production, none aimed at a shared layer. | Configurations, enforcement points, coverage, and whether the controls can be bypassed. |
| 18 | Is Claude required, preferred, or one supported provider? | **Partially answered** | The sponsor talks in Claude model names throughout and reasons about per-function model selection within the Claude family. No portability requirement stated. | Confirm. This gains weight because per-function routing is now a named product capability. |
| 33 | What Anthropic plan tier, and is the private plugin marketplace available? | **Unanswered** | Gates the distribution spike in `technical-stack.md`. | Plan tier, seats for the eight builders, admin console access, telemetry export. |
| 36 | What would count as evidence that the audit-only model is right after all? | **Answered - proposed** | Time from plan to first working application, measured before and after the first extractions. If it goes the wrong way, the core is wrong for this team. | Agreement that this is a fair test. |
| 44 | Can an MCP server be distributed to all eight builders' Claude environments without per-person setup? | **Unanswered** | The understanding layer reaches builders through MCP, which is the mechanism that keeps it invisible. If distribution requires each person to configure something, the invisibility constraint is compromised at the point of installation. | Confirm how builder environments are managed, and whether org-level MCP configuration is available on the current plan. |
| 45 | Is there anything in the repositories that a reader could not determine without a connector? | **Unanswered** | The population method is read-through first. This question is the trigger for adding a connector. Deployment reality is the expected first exception, since whether an application is live is not reliably in source. | List the questions the repositories cannot answer, before building anything to answer them. |
| 46 | What standards should a new application follow, and where are they written down? | **Unanswered** | `standards_get` is one of the six MCP tools, and it assumes such standards exist in retrievable form. CODEOWNERS, MERGE-GATE and skill-lint are reported as in production, which is a partial answer. | Identify what exists as written convention versus what lives in people's heads. |

---

## P1 - Station

New scope from the call. The sponsor's instruction: "Don't make any proposals there. Just know that I probably do want some time in the first three weeks or so."

| ID | Question | Status | What is needed |
| --- | --- | --- | --- |
| 37 | What is the org structure after the split? | **Answered - verified** | Capital Factory is the venture fund. Station Austin is the coworking space, events, mentoring, and memberships. Station Northwest Arkansas and Station DC also exist. |
| 38 | What is Station's AI maturity? | **Answered - verified** | "They are only using Claude as a chatbot. They're not building apps at all. They don't even have GitHub." |
| 39 | What repeated, human-intensive work exists at Station? | **Unanswered** | Interviews with Station Austin leadership first. Two candidates the sponsor named: event programming, with SXSW planning cited as enormous, and grant writing, both finding opportunities and assisting with the writing. |
| 40 | Who at Station Austin should be interviewed, and when? | **Unanswered** | Names and a window inside the first three weeks. |
| 41 | Does the empty-estate case break any v0 requirement? | **Unanswered** | Every component must be valid against zero capabilities. A registry with nothing in it, a sweep with nothing to find, and a discovery query that always returns no match must all be correct states. Station is a stronger test of the architecture than Capital Factory is. |
| 42 | What does "the scaffold" include? | **Partially answered** | The sponsor: "We can go get them set up on GitHub using the right way. We can get them set up with whatever the architecture is. We get their OS set up. Boom, bring it over there. Don't bring all the apps and all this stuff, but just bring the environment." Define the minimum portable set. |

---

## Out of scope: identity, security, and systems administration

The sponsor raised this himself and assigned it elsewhere:

> "Josh Baer was basically the system admin for everything. We've recovered that. We got access to his YubiKey and his 1Password account. And so we've moved that over to his chief of staff. That's not the right way to do it. I am not sure that you are the right person to help me. I think there is probably an IT person that I need to bring in."

He also declined an introduction for now: "hang tight on that. Don't make an introduction, just know that it's on my mind."

The previous version of this document carried six questions here (19 through 24) and the previous 90-day plan carried an IAM workstream. Both are withdrawn. What remains is an obligation to record, not to remediate.

**Carried as a Milestone 1 handoff for the incoming IT consultant, observation only:**

- Personal identities holding system-admin rights on business-critical accounts, including the sponsor's own 1Password identity on roughly ten key accounts, which he named as a concern.
- Credentials embedded in application code or local environments, found incidentally while mapping the tool layer.
- Applications running on employee machines, which is a platform coverage gap as well as an access one.
- Systems reached by an application whose administrator or long-term owner is unknown.

**Still needed from Capital Factory, because the product depends on it and the IT consultant does not gate it:**

| ID | Question | Status | What is needed |
| --- | --- | --- | --- |
| 22 | May prompts, responses, tool inputs, and traces be stored? | **Unanswered** | A decision from a named data-policy owner, by data classification. This gates how deep the telemetry in the console can go. |
| 43 | Who is the data-policy owner? | **Unanswered** | A name. |

---

## P2 - Budget

| ID | Question | Status | What is needed |
| --- | --- | --- | --- |
| 30 | What non-labour platform budget is available? | **Unanswered** | Not raised on the call. Rough shape is $100 to $400 a month self-hosted, or $500 to $2,000 managed. See `technical-stack.md`. |
| 32 | What usage and cost data supports the model-routing case? | **Unanswered** | AI provider usage exports and account mappings. Without these, the per-function routing saving in Milestone 5 cannot be measured, only asserted. |

---

## The first working session

Six items, in this order.

1. Approve read-only access to the five repositories and name the technical contact. Nothing starts without this.
2. Confirm the architecture position in `architecture-decision.md`, or argue with it. The reasoning is written down specifically so it can be argued with.
3. Name the Station Austin contacts and a window in the first three weeks.
4. Confirm the tool layer is genuinely unknown, so that Milestone 1 is scoped to find out rather than to confirm.
5. Name a data-policy owner.
6. Confirm Milestone 1 and its acceptance test.

Everything else waits for evidence.

---

## Initial read-only access requested

Time-bounded, least-privilege, individually assigned, MFA-protected. No secret values and no unrestricted production data.

| System | Initial access | Purpose | Status |
| --- | --- | --- | --- |
| GitHub | Organization and repository read | Verify the estate, dependencies, workflows, owners, controls | Not requested |
| Vercel and other hosting | Project viewer | Map deployments, environments, runtime ownership | Not requested |
| AI provider accounts | Usage and billing viewer | Map models, tokens, and cost for the routing case | Not requested |
| Monitoring platforms | Viewer | Assess errors, traces, and alert ownership | Not requested |

1Password and Google Workspace admin review have been dropped from this request. They belong to the IT and security engagement, not this one.

---

## Minimum audit outputs

1. Verified estate register with live-versus-dormant status.
2. Tool-layer map: how every application reaches every system.
3. Ownership map for priority applications.
4. Access and credential risk handoff for the incoming IT consultant, observation only.
5. Station opportunity notes, no proposals.
6. Evaluation coverage baseline, which sets the extraction cap.

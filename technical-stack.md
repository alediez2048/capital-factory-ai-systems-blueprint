# Proposed Technical Stack

Status: Candidate analysis - component selections are not final; the overall posture is proposed for confirmation as ADR-009
Last updated: August 20, 2026
Change note: Restructured around a single recommended posture. An earlier draft presented an end-to-end agent platform as a co-equal route; this draft explains up front why that route is not proposed, and retains the platform survey as evidence that it was considered rather than overlooked. Vendor capability claims are Reported from vendor documentation and industry comparisons as of August 2026 and require verification during discovery.

## The posture, and why

This document proposes one route: keep GitHub and Vercel as the estate, adopt best-of-breed components for the commodity layers (gateway, traces, errors, scheduled jobs), and build only the thin layer that is genuinely specific to Capital Factory. It deliberately does not propose migrating the estate into an end-to-end agent platform, and it deliberately does not propose building the commodity layers from scratch. The reasoning:

**Capital Factory has already stated its preference in its own materials.** The July 22 architecture survey concludes that a plugin marketplace, CODEOWNERS review, a MERGE-GATE policy, skill-lint, and an evaluation suite are all in production today and "have simply never been aimed at a shared capability layer." An organization inclined to buy a platform does not write that sentence; it is an inventory of machinery the organization built for itself and intends to keep using. The proposal here aims that machinery rather than replacing it.

**The structure of the mandate is a build signal.** The August 6 operating mandate budgets for a full-time operator-engineer and states no separate platform budget. That is the spend profile of an organization that intends to assemble and own its system, not procure one. A platform migration would also consume most of the 90 days re-authoring 51 existing items into a vendor's format before any governance value appeared, which is incompatible with the mandate's Day-90 proof.

**The estate's format decides most of the question mechanically.** The 51 items are Claude Code skills in GitHub repositories. Every general agent platform surveyed (Dust, Microsoft Copilot Studio, Google Gemini Enterprise, Retool, n8n-class automation) requires rebuilding those artifacts inside its own frame. The single exception is Anthropic's own private plugin marketplace, which consumes the existing format; it is retained below as a Phase 1 spike inside this posture, not as a platform migration.

**Control and data posture rule out the rest.** The mandate's central concerns are recoverable access, institutional ownership, and sensitive LP and investor data. Routing that data and its guardrails through a new end-to-end vendor answers the provider-neutrality question (ADR-002) permanently and by default, and moves compliance-bearing logic inside a product Capital Factory does not control.

**Scratch-building the commodity layers fails the same test from the other side.** A hand-rolled gateway, trace store, or job queue is a 52nd internal application that itself needs an owner after Day 90. The mandate explicitly requires a maintainable system after the engagement; in 2026 these layers are commodity open source or inexpensive managed services, and building them recreates undifferentiated plumbing at the cost of the differentiated work.

What remains genuinely Capital Factory's to build is short and portable across every vendor decision below: the skill contract format and generated catalog, the eval suites wired into MERGE-GATE as release gates, the safe-database-writes guardrail and its universal adoption, the autonomy ladder as measured policy, person-config separation so skills are shared and people are not, and the person-to-keys-to-runtimes dependency map behind the revocation preview. That list is the engineering scope of the 90 days.

Conditions that would reopen this posture, to be recorded in ADR-009: a decision to leave Claude Code as the authoring environment; headcount growth that makes per-seat platform pricing cheaper than operator maintenance; or a security mandate requiring a vendor-managed boundary rather than a self-managed one.

## Decision standard

Component choices within this posture should be evaluated against:

- Fit with Capital Factory's current estate.
- Security and data-handling requirements.
- Ease of operation after the 90-day engagement.
- Integration with GitHub, Vercel, Google Workspace, 1Password, and existing applications.
- Portability and vendor lock-in.
- Total cost, including maintenance burden.
- Support for application-level ownership, health, AI usage, and evaluation evidence.

## Platforms considered and set aside

Recorded so the decision is documented as made, not missed. All claims Reported, August 2026.

| Platform | What it offers | Why it is not proposed |
| --- | --- | --- |
| Dust | Company agents over shared data, multi-model, spaces and permissions | Built for knowledge-worker agents authored in its own builder; the repo-based, code-heavy estate would be rebuilt, not adopted |
| Microsoft Copilot Studio | Agent building and governance inside Microsoft 365 | CF runs Google Workspace; the estate is not Microsoft-shaped; adoption implies a platform shift far beyond the mandate |
| Google Gemini Enterprise | Agent gallery, Workspace-native identity and governance | Identity alignment is attractive, but agents are authored in Google's frame; the 51 items would be re-created from scratch |
| Retool (Agents) | Agents plus internal-tool UI, RBAC, audit | Same re-authoring problem; its UI strengths overlap with the thin catalog this posture builds cheaply |
| n8n / Zapier-class | Workflow automation with AI steps, many connectors | Automation, not governance; no answer for judgment-bearing skills with eval and approval requirements |

One platform-level capability is retained inside the recommended posture: the Claude Enterprise private plugin marketplace (Reported: org-scoped marketplaces, per-user provisioning, auto-install, admin controls, OpenTelemetry usage and cost telemetry, private GitHub repositories as plugin sources). Because it distributes the format Capital Factory already writes, it is a candidate distribution channel, not a platform migration. It enters as a Phase 1 spike with explicit verification questions below.

## Stack map by capability

### Skill distribution and governance

The question ADR-001 must answer: how does a versioned skill get from the core repository into a consuming app?

- Claude private plugin marketplace - near-native for the existing estate; would collapse most of ADR-001 into configuration. Verify in the Phase 1 spike: version pinning semantics, rollback behavior, whether consumers can lag a release, telemetry granularity, and the lock-in this adds to ADR-002.
- Git-native (submodules, subtree, or tagged releases pulled in CI) - zero new vendors, full control, works today; weaker ergonomics and nothing enforces upgrade hygiene without added tooling. The fallback if the spike disappoints.
- Package registry (npm/pip private packages) - clean versioning and lockfiles; awkward for markdown-and-prompt skill content, natural for the deterministic tool layer.
- Likely hybrid: tools as packages, skills as marketplace plugins or tagged repo content, one manifest per consumer recording versions.

### Catalog and control plane (the Core surface)

- Build thin and GitHub-native first (the ADR-003 position): a catalog generated from skill manifests, rendered as the Core UI shown in this repository's mockup. Cheapest, exactly fits the taxonomy, owned code.
- Port - SaaS developer portal with a flexible data model that could represent apps/skills/tools/systems; scorecards map to production-readiness. Hold as the managed fallback if the thin catalog proves insufficient.
- Backstage - open source but heavy to self-host and shaped around microservice catalogs; poor effort-to-value for eight builders.
- Cortex / OpsLevel - enterprise engineering portals priced and shaped for much larger orgs; not recommended at this scale.

### AI gateway (attribution, budgets, keys)

- Direct Anthropic with workspace controls - if Claude-only is affirmed under ADR-002, Anthropic's workspace separation, spend caps, and usage reporting may satisfy attribution without any gateway. Simplest architecture; verify per-app granularity meets AI-001/AI-002.
- LiteLLM (self-hosted) - virtual keys scoped per app/team/user, budgets, rate limits, spend tracking, model aliases; open source, runs inside CF's boundary. The candidate if a gateway is needed at all. Cost: one more production service to operate.
- Portkey - managed gateway with guardrails and retries; prompt traffic transits a vendor, which the ADR-005 data decision may forbid.
- Cloudflare AI Gateway - lightweight logging and spend visibility; thinner per-app key governance than LiteLLM.
- OpenRouter-class marketplaces - not recommended for production paths; investor-data prompts routed through a third-party model marketplace conflicts with the data posture.

Decision rule: adopt a gateway only against a verified control need. Claude-only affirmed means test direct Anthropic first.

### AI observability and evaluation

- Langfuse - open source, self-hostable, traces plus evals plus prompt management, OpenTelemetry-friendly, integrates with LiteLLM. Default under a restrictive data policy because content stays inside CF infrastructure.
- Arize Phoenix - open source, OTel-native traces and evals; credible alternative on the same terms.
- Braintrust - eval-first managed platform with strong dataset and regression workflows; attractive if the data policy permits a vendor.
- LangSmith - polished but strongest inside the LangChain ecosystem CF does not use.
- Sentry stays for application errors regardless; it answers a different question.

Decision rule: ADR-005 gates this choice. Until a data-policy owner approves content capture, run metadata-only telemetry through OpenTelemetry, which every candidate ingests, and defer the platform commitment.

### Runtime for scheduled and headless workloads

Vercel remains for web applications. The gap is durable scheduled jobs with retries and human-approval pauses, required to move workloads off employee machines.

- Railway / Render - simplest managed homes for small services and cron; the default for first migrations.
- Trigger.dev / Inngest - durable orchestration with retries, queues, long waits, and human-in-the-loop steps as first-class citizens. Notable: the approval gate in safe-database-writes is exactly a durable wait, and these platforms implement the hardest part of that pattern.
- Modal - serverless Python with cron; strong if workloads are Python-heavy.
- Google Cloud Run - scale-to-zero containers under the identity plane CF already uses; more surface than Railway, better IAM alignment.
- Temporal and Kubernetes - unjustified at this scale; explicitly out.

### Identity, secrets, scanning, uptime

| Capability | Leading candidate | Alternatives | Current position |
| --- | --- | --- | --- |
| Source control | GitHub | Existing estate | Reported existing system; policies require audit |
| CI/CD | GitHub Actions plus provider-native deployment | Vercel workflows, cloud-native pipelines | Likely; confirm current workflows |
| Identity | Existing Google Workspace SSO + MFA | Okta, Entra ID | Unknown until IAM discovery; no evidence a dedicated IdP is needed |
| Secrets | 1Password plus hosting/cloud secret stores | Vault, cloud secret manager | Existing 1Password reported; machine-secret model requires review |
| Security scanning | GitHub-native controls (secret scanning, Dependabot, push protection) | Semgrep, Snyk, Trivy | Budget and repository risk dependent |
| Error monitoring | Sentry | Datadog, Grafana Cloud | Candidate; check existing contracts |
| Telemetry standard | OpenTelemetry | Vendor-native instrumentation | Recommended for portability; also the format Anthropic's enterprise telemetry emits |
| Uptime | Existing monitoring if available | Better Stack, UptimeRobot | Select after observability audit |
| Infrastructure definition | Repository-managed configuration | Terraform, Pulumi | Introduce only where it reduces recovery and drift risk |

## Indicative non-labor cost sketch

Rough monthly orders of magnitude, stated to inform the open budget question (question 30 in the discovery register). Verify against real quotes; excludes model token spend.

- Lean self-host posture: LiteLLM and Langfuse self-hosted on one small managed host, Railway or Render for jobs, Sentry team tier, GitHub/Vercel as-is. Roughly $100-$400 per month plus operator attention.
- Managed posture: hosted observability, Trigger.dev or Inngest, Sentry, portal product later. Roughly $500-$2,000 per month with less operational burden after Day 90.

The difference is not the dollars; it is who maintains the plumbing after the engagement. The Day-90 staffing recommendation should drive this choice.

## Recommended initial position

1. Preserve before replacing. GitHub and Vercel stay; map and stabilize before any migration.
2. Spike the Claude private plugin marketplace in Phase 1 against existing skills. If distribution, provisioning, and telemetry hold up, ADR-001 becomes configuration rather than construction; record the ADR-002 lock-in tradeoff explicitly.
3. Build the catalog thin and GitHub-native; hold Port as the managed fallback.
4. Adopt a gateway only against a verified control need; if Claude-only is affirmed, test direct Anthropic workspace controls first.
5. Run metadata-only observability through OpenTelemetry now; choose the eval/trace platform after the data-policy decision, with self-hosted Langfuse as the default under a restrictive policy.
6. Use a durable-jobs platform for scheduled workloads and approval gates rather than hand-rolling queues; Railway or Render for plain services.
7. Avoid Kubernetes, enterprise IDPs, Temporal, and end-to-end agent platforms, for the reasons recorded at the top of this document.

## Architecture decisions to record

- ADR-001: Shared capability distribution model - including the managed plugin-marketplace option.
- ADR-002: Claude-specific or provider-neutral AI access - coupled to the marketplace and gateway choices.
- ADR-003: Application catalog and control-plane implementation.
- ADR-004: Secrets and machine-identity model.
- ADR-005: Observability, content capture, and retention.
- ADR-006: Production hosting and environment strategy, including the durable-jobs question.
- ADR-007: Evaluation platform and release gates.
- ADR-008: Repository topology and ownership.
- ADR-009: Platform posture - the assemble-and-build-thin route this document proposes, recorded with the rejected end-to-end platforms, the rejected scratch-build, and the reopening conditions stated above.

## Required evidence before final selection

1. Repository and language inventory.
2. Runtime and hosting inventory.
3. Current contracts and platform spending.
4. Identity and secrets architecture.
5. Data classifications and provider policies.
6. Existing monitoring and evaluation tooling.
7. Post-90-day maintenance capacity.
8. Approved reference application and performance expectations.
9. Marketplace spike result: can an existing skill be packaged, distributed, version-pinned, and rolled back through the Claude private marketplace without loss of control.
10. Anthropic enterprise telemetry sample: whether per-app usage and cost attribution meets AI-001/AI-002 without a gateway.

## Sources consulted

Vendor documentation and industry comparisons, August 2026. All claims Reported, not verified: Anthropic Cowork/plugins enterprise announcement and Claude Enterprise pages; LiteLLM proxy documentation (virtual keys, budgets, spend tracking); Langfuse documentation and 2026 observability comparisons (LangSmith, Braintrust, Arize Phoenix); internal developer portal comparisons (Port, Backstage, Cortex, OpsLevel); AI gateway comparisons (LiteLLM, Portkey, Cloudflare AI Gateway, OpenRouter); agent-platform comparisons (Dust, Copilot Studio, Gemini Enterprise, Retool); AI app hosting and background-job platform comparisons (Railway, Render, Modal, Trigger.dev, Inngest).

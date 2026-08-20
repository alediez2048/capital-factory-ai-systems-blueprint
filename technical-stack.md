# Proposed Technical Stack

Status: Candidate analysis - no vendor selection is final

## Decision standard

Technology choices should be evaluated against:

- Fit with Capital Factory's current estate.
- Security and data-handling requirements.
- Ease of operation after the 90-day engagement.
- Integration with GitHub, Vercel, Google Workspace, 1Password, and existing applications.
- Portability and vendor lock-in.
- Total cost, including maintenance burden.
- Support for application-level ownership, health, AI usage, and evaluation evidence.

## Preliminary stack map

| Capability | Leading candidate | Alternatives | Current position |
| --- | --- | --- | --- |
| Source control | GitHub | Existing estate | Reported existing system; policies require audit |
| CI/CD | GitHub Actions plus provider-native deployment | Vercel workflows, cloud-native pipelines | Likely; confirm current workflows |
| Application hosting | Preserve Vercel where appropriate; managed compute for server workloads | Cloud Run, AWS, Azure, Railway, Render | Do not standardize before runtime inventory |
| Developer portal/catalog | Port | Backstage, lightweight GitHub-native catalog | Candidate; start lightweight and validate need |
| AI gateway | LiteLLM | Direct Anthropic access, cloud AI gateway, custom proxy | Candidate; depends on Claude policy and data constraints |
| AI observability/evaluation | Langfuse | Braintrust, Arize Phoenix, cloud-native platforms | Candidate; content logging requires data approval |
| Error monitoring | Sentry | Datadog, Grafana Cloud, cloud-native monitoring | Candidate; check existing contracts |
| Telemetry standard | OpenTelemetry | Vendor-native instrumentation | Recommended for portability where practical |
| Secrets | 1Password plus hosting/cloud secret stores | Vault, cloud secret manager | Existing 1Password reported; machine-secret model requires review |
| Identity | Existing Capital Factory identity provider | Google Workspace, Okta, Entra ID | Unknown until IAM discovery |
| Security scanning | GitHub-native controls | Semgrep, Snyk, Trivy, managed security suite | Budget and repository risk dependent |
| Uptime | Existing monitoring if available | Better Stack, Grafana, UptimeRobot, Datadog | Select after observability audit |
| Infrastructure definition | Repository-managed configuration | Terraform, Pulumi, provider configuration | Introduce only where it reduces recovery and drift risk |

## Recommended initial position

### Preserve before replacing

GitHub and Vercel are already part of the reported estate. The first phase should map and stabilize their usage rather than initiate a broad platform migration.

### Add a shared-core repository only after validating the runtime model

The architecture deck recommends a central repository, but the implementation depends on whether skills are files, packages, services, MCP tools, or a hybrid. Repository creation should follow this decision.

### Use a gateway only if it solves a verified control problem

LiteLLM could provide application attribution, budgets, provider flexibility, and centralized credentials. It also introduces another production dependency. Direct Anthropic access may be simpler if Capital Factory intentionally standardizes on Claude and existing provider controls meet the requirements.

### Adopt observability with data minimization

OpenTelemetry, Sentry, and an AI observability platform can expose health, errors, latency, tokens, cost, tool calls, and evaluations. Prompt and response content should remain disabled or redacted until Capital Factory approves data classification and retention policies.

### Avoid unnecessary Kubernetes

Nothing in the current evidence requires Kubernetes. Managed hosting, serverless platforms, or containers on managed compute are more appropriate until workload scale and control requirements justify cluster operations.

## Architecture decisions to record

- ADR-001: Shared capability distribution model.
- ADR-002: Claude-specific or provider-neutral AI access.
- ADR-003: Application catalog and control-plane implementation.
- ADR-004: Secrets and machine-identity model.
- ADR-005: Observability, content capture, and retention.
- ADR-006: Production hosting and environment strategy.
- ADR-007: Evaluation platform and release gates.
- ADR-008: Repository topology and ownership.

## Required evidence before final selection

1. Repository and language inventory.
2. Runtime and hosting inventory.
3. Current contracts and platform spending.
4. Identity and secrets architecture.
5. Data classifications and provider policies.
6. Existing monitoring and evaluation tooling.
7. Post-90-day maintenance capacity.
8. Approved reference application and performance expectations.


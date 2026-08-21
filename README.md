# Capital Factory AI Systems Blueprint

Status: Working draft for discovery and proposal development  
Prepared by: Jorge Alejandro Diez  
Last updated: August 20, 2026

## Purpose

This repository organizes the current understanding of Capital Factory's AI application estate and defines a practical path toward a governed, reusable, production-ready shared AI core.

It is intended to support three outcomes:

1. Run a focused discovery and access audit with Capital Factory.
2. Convert verified findings into production requirements and architecture decisions.
3. Produce a credible 90-day implementation proposal and executive presentation.

## Current position

Capital Factory has already completed meaningful architectural analysis. Provided materials report:

- 51 packaged items across five GitHub repositories.
- 31 applications and approximately 16 reusable capabilities.
- Approximately 10 capabilities implemented more than once.
- 40 of 51 items described as strongly documented.
- No central capability repository.
- One identified evaluation suite.
- A proposed four-layer architecture separating applications, shared skills, deterministic tools, and systems of record.
- 48 business systems in the systems and administrative ownership register, with ownership transfers and long-term ownership still requiring attention.

The principal gap is not a lack of AI ideas. It is the transition from documented strategy and distributed experiments to an accountable, governed, observable operating system.

## Evidence labels

All important statements should use one of these labels:

- **Verified** - confirmed through an authoritative system, repository, or responsible owner.
- **Reported** - stated in Capital Factory-provided materials but not independently confirmed.
- **Proposed** - a recommendation for discussion.
- **Unknown** - information still required.

Unless explicitly marked verified or approved, architecture and technology selections in this repository remain provisional.

## Documents

| Document | Purpose |
| --- | --- |
| [discovery.md](discovery.md) | Essential questions and access needed for the initial audit |
| [requirements.md](requirements.md) | Proposed production, security, AI, and operational requirements |
| [system-design.md](system-design.md) | Conceptual target architecture and system boundaries |
| [technical-stack.md](technical-stack.md) | Platform posture (assemble and build thin), rejected alternatives, candidate components, and validation gates |
| [90-day-proposal.md](90-day-proposal.md) | Proposed phased engagement and measurable outcomes |
| [sources/README.md](sources/README.md) | Register of materials used to build the blueprint |

## Scope boundaries

This repository is not currently:

- An approved Capital Factory production specification.
- The future shared-core source repository.
- A complete inventory of every application, system, identity, integration, or data flow.
- Authorization to access or modify Capital Factory systems.
- A commitment to a specific vendor or cloud platform.

## Immediate next decision

The next step is a focused discovery session with Capital Factory to confirm the mandate, obtain least-privilege read-only access, verify the five-repository inventory, and select the first production proof point.


# TrustRegistry

**Status:** Documentation-first — no production code until design is agreed.

TrustRegistry is a regulated-industry trust platform that enables organisations to assemble, preserve, and selectively disclose integrity-proven evidence packages—so regulators, auditors, and partners can independently review the same material and record their own trust assertions, without the platform declaring truth.

## Related products

**AuditorsVault** is a separate product: PostgreSQL-native temporal audit infrastructure for reconstructable, tamper-evident database history. TrustRegistry may consume AuditorsVault as an implementation component for immutable history (FP-060), but the products have distinct repositories, visions, and buyers. See [governance/ArchitectureDecisionLog.md](governance/ArchitectureDecisionLog.md) ADR-020.

| Product | Repository | Focus |
|---------|------------|-------|
| **TrustRegistry** | This repo ([github.com/brormuller/TrustRegistry](https://github.com/brormuller/TrustRegistry)) | Inter-org trust, evidence packages, trust assertions |
| **AuditorsVault** | [AuditorsVault/site](https://github.com/AuditorsVault/site) | PostgreSQL change capture and database history |

TrustRegistry and AuditorsVault are **peer products**—separate top-level GitHub repositories, not nested under one another.

## How to read this repository

This project follows a **documentation-first** development philosophy. No application code in `src/` should be written until the relevant design documents reach **Agreed** status.

### Development order

1. Platform Principles
2. Vision
3. Problem Statement
4. Domain Model
5. Requirements
6. Architecture
7. UI/UX
8. APIs
9. Security
10. Implementation

If you are tempted to skip ahead, check prerequisites in the governing documents first.

### Document locations

| Area | Purpose |
|------|---------|
| [governance/](governance/) | Constitutional documents, ADRs, questions, risks |
| [docs/](docs/) | Domain model, requirements, architecture (later) |
| [ui/](ui/) | User journeys and design (when requirements are stable) |
| [src/](src/) | Application code (empty until agreed design) |

### Numbering convention

All significant items use increment-of-10 numbering:

- **FP-010** — Platform Principles
- **PS-010** — Problem statement themes
- **DM-010** — Domain model elements
- **FR-010 / NFR-010** — Requirements
- **ADR-010** — Architecture decisions
- **Q-010** — Open questions
- **RISK-010** — Risks

### Document status

Every document carries a **Status**: `Draft` | `Review` | `Agreed`.

Only **Agreed** documents gate implementation work.

### Start here

1. [docs/TrustRegistryFoundation.md](docs/TrustRegistryFoundation.md) — architecture foundation and plan
2. [governance/PlatformPrinciples.md](governance/PlatformPrinciples.md) — constitutional document
3. [governance/ProductVision.md](governance/ProductVision.md) — vision and beachhead
4. [governance/Questions.md](governance/Questions.md) — open uncertainties
5. [governance/Risks.md](governance/Risks.md) — known risks

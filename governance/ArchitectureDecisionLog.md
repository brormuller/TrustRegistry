# Architecture Decision Log

**Document ID:** GOV-ADR  
**Status:** Draft  
**Last updated:** 2026-06-28

Record of significant architecture and design decisions. Each ADR is immutable once accepted; supersession requires a new ADR.

---

## Index

| ID | Title | Status | Date |
|----|-------|--------|------|
| ADR-010 | Evidence package as unit of disclosure and review | Accepted | 2026-06-28 |
| ADR-020 | TrustRegistry and AuditorsVault as separate products | Accepted | 2026-06-28 |
| ADR-030 | Multi-client architecture — API-first, thin clients | Accepted | 2026-06-28 |

---

## ADR-010 — Evidence package as unit of disclosure and review

**Status:** Accepted  
**Date:** 2026-06-28  
**Deciders:** Product architecture (initial foundation)

### Context

TrustRegistry must support multiple organisations independently reviewing the same evidence while operating as multi-tenant SaaS (FP-070). Two naive models conflict:

1. **Shared vault model** — reviewers browse custodian tenants' evidence stores. Violates FP-030, creates privacy risk (RISK-030), and blurs tenant boundaries.
2. **Ad hoc file sharing** — email and drives without boundary objects. Fails PS-020, PS-010; no integrity proofs or structured assertions.

The domain requires a clear **boundary object** for disclosure, export, integrity verification, and trust assertions.

### Decision

The **Evidence Package** (specific version) is the unit of:

- Assembly and publication by custodians
- Scoped **Disclosure** to reviewers
- **Integrity proof** computation and optional anchoring
- **Trust assertion** attachment
- **Export** and offline verification

Reviewers receive access to defined evidence packages—not blanket access to custodian vaults or tenant-wide data stores.

Cross-tenant interaction occurs only through explicit Disclosure records on package versions.

### Consequences

**Positive:**

- Aligns with FP-030, FP-040, FP-070, FP-080
- Portable, integrity-proven exports are natural
- Multi-party review on identical material is explicit
- Simplifies security model: authorisation at package + disclosure level

**Negative / trade-offs:**

- Custodians must assemble packages deliberately—cannot rely on "give auditor login to everything"
- Package versioning and supersession must be well designed (FP-060)
- Partial disclosure (subset of items) adds complexity; may defer subset scoping in v1

**Open:**

- Q-030: in-platform reviewer accounts vs export-only review
- Q-060: assertion type structure

### Related

- [DomainModel.md](../docs/DomainModel.md) — DM-040
- [PlatformPrinciples.md](PlatformPrinciples.md) — FP-030, FP-040, FP-070, FP-080
- [Terminology.md](Terminology.md) — Evidence Package, Disclosure

---

## ADR-020 — TrustRegistry and AuditorsVault as separate products

**Status:** Accepted  
**Date:** 2026-06-28  
**Deciders:** Product architecture

### Context

Two related but distinct product directions emerged:

1. **TrustRegistry** — regulated-industry trust platform: evidence packages, scoped disclosure, independent trust assertions, multi-tenant SaaS.
2. **AuditorsVault** — PostgreSQL temporal audit infrastructure: reconstructable, tamper-evident database change history (existing work at [github.com/AuditorsVault/site](https://github.com/AuditorsVault/site)).

Merging them into one product or one repository would:

- Confuse buyers (compliance owners vs database engineers)
- Blur domain models (evidence package vs row-level history)
- Risk RISK-010 (GRC tool) and RISK-080 (boundary blur)

### Decision

- **TrustRegistry** is the regulated trust platform. This repository is its home—a **peer** top-level GitHub repository (not nested under AuditorsVault).
- **AuditorsVault** remains the PostgreSQL database history tool in its own repository ([AuditorsVault/site](https://github.com/AuditorsVault/site)).
- TrustRegistry **may depend on** AuditorsVault for operational immutability (FP-060)—e.g. audit tables, change history—but AuditorsVault is **not** the TrustRegistry product and does not implement disclosure, packages, or trust assertions.
- Integration boundary deferred to architecture phase (Q-080).

### Consequences

**Positive:**

- Clear positioning and sales narrative for each product
- AuditorsVault research continues without forcing premature trust-domain modeling
- TrustRegistry architecture can adopt AuditorsVault when FP-060 implementation is designed

**Negative / trade-offs:**

- Two repos to maintain; need explicit versioning/integration story later
- Must avoid duplicate integrity/history logic without clear layering

**Open:**

- Q-080: integration pattern (library, service, adapter)

### Related

- [ProductVision.md](ProductVision.md) — Related products
- [Questions.md](Questions.md) — Q-000 (answered), Q-080
- [Risks.md](Risks.md) — RISK-080

---

## ADR-030 — Multi-client architecture (API-first, thin clients)

**Status:** Accepted  
**Date:** 2026-06-28  
**Deciders:** Product architecture

### Context

TrustRegistry is intended as both a **web application** and a **mobile application**. Without an explicit client architecture decision, teams typically:

1. Embed business rules in the first client built (usually web)—forcing mobile to catch up or diverge.
2. Build channel-specific APIs—breaking NFR-040 (integrity verifiable outside UI) and FP-050 (consistent assertion attribution).
3. Commit to full feature parity on both channels before journeys are validated—inflating v1 scope (RISK-090, FP-090).

NFR-040 already requires integrity proofs verifiable independently of any UI. ADR-010 defines the server-side boundary object (evidence package). A client architecture decision completes the picture before Phase 1 API design.

### Decision

- TrustRegistry exposes a **single canonical platform API** consumed by **peer clients** (web, mobile, and future channels).
- **Clients are thin:** presentation, input validation for UX, and session handling only. They do not implement trust logic, integrity computation, disclosure authorisation, or assertion semantics.
- **Business rules live server-side:** package publication, disclosure, integrity proofs, trust assertions, and audit logging are API responsibilities.
- **No client-specific server behaviour** unless justified by a future ADR (e.g. mobile push registration is an capability, not a fork of domain rules).
- **Channel scope is product-defined, not parity-assumed:** web and mobile may ship different journey subsets in v1 per Q-090–Q-093; the API supports all agreed capabilities regardless of which client exposes them first.

### Consequences

**Positive:**

- Aligns with NFR-040, FP-010, FP-050, FP-080
- Web-first MVP (Phase 2) does not block mobile later
- Security model centralised: auth, authorisation, audit at API layer
- API.md becomes the contract both clients implement against

**Negative / trade-offs:**

- API design must be complete enough for mobile constraints (pagination, partial reads, file upload patterns) even if mobile ships later
- Mobile-specific concerns (push, biometrics, secure enclave, offline—Q-092) must be addressed in Security and Architecture, not bolted on
- Requires discipline to resist "just this once" logic in web frontend

**Open:**

- Q-090: parity vs role-based channel scope
- Q-091: mobile-critical journeys
- Q-092: offline mobile
- Q-093: native vs hybrid vs responsive web

**Deferred (Phase 1):**

- Client technology stack selection
- Wireframes per channel
- Push notification provider

### Related

- [ProductVision.md](ProductVision.md) — Multi-channel product
- [Requirements.md](../docs/Requirements.md) — NFR-100
- [Questions.md](Questions.md) — Q-090–Q-093
- [Risks.md](Risks.md) — RISK-090

---

## ADR template (for future use)

```markdown
## ADR-0XX — Title

**Status:** Proposed | Accepted | Superseded by ADR-0YY
**Date:** YYYY-MM-DD

### Context
### Decision
### Consequences
```

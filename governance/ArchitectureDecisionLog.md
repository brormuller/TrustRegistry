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

- **TrustRegistry** is the regulated trust platform. This repository (`C:\GitHub\TrustRegistry`) is its home.
- **AuditorsVault** remains the PostgreSQL database history tool in its own repository.
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

## ADR template (for future use)

```markdown
## ADR-0XX — Title

**Status:** Proposed | Accepted | Superseded by ADR-0YY
**Date:** YYYY-MM-DD

### Context
### Decision
### Consequences
```

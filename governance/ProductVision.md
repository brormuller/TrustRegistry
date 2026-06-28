# Product Vision

**Document ID:** GOV-VISION  
**Status:** Draft  
**Last updated:** 2026-06-28

---

## One-line vision

TrustRegistry enables regulated organisations to assemble, preserve, and selectively disclose integrity-proven evidence packages—so regulators, auditors, and partners can independently review the same material and record their own trust assertions, without the platform declaring truth.

---

## What we are building

**Digital trust infrastructure**—not a blockchain product, not a compliance dashboard, not a regulator-of-record, not a database audit tool.

The product helps organisations:

1. **Establish** trust in entities through structured evidence
2. **Preserve** that evidence with tamper-evident history
3. **Prove** integrity to third parties through portable packages and cryptographic proofs
4. **Enable** independent reviewers to record their own trust assertions

Blockchain, when implemented, is an **interchangeable publishing provider** for optional external anchoring—not a core dependency.

---

## Multi-channel product (web and mobile)

TrustRegistry is a **multi-channel product** available as a **web application** and a **mobile application**.

- Web and mobile are **peer clients** on the same trust domain—they do not define business rules.
- The **platform API is canonical**; clients are thin presentation and interaction layers (ADR-030).
- **Full feature parity across channels is not assumed for v1** (FP-090). Channel scope is role- and journey-based until Q-090–Q-093 are resolved.
- Integrity proofs, trust assertions, disclosure, and audit events remain **server-side** regardless of client (NFR-040, NFR-100).

**Working hypothesis (Q-090):** Web is primary for custodian and admin workflows; mobile may lead for reviewer notification, package review, and assertion capture—subject to beachhead validation.

---

## Related products

### AuditorsVault (separate product)

[AuditorsVault](https://github.com/AuditorsVault/site) is PostgreSQL temporal audit infrastructure: reconstructable, inspectable, tamper-evident **database history**. It is not TrustRegistry and does not model evidence packages, disclosure, or trust assertions.

TrustRegistry will **almost certainly use** AuditorsVault (or similar mechanisms) internally for immutable operational history—but that is an implementation choice, not the product definition. See ADR-020.

| | TrustRegistry | AuditorsVault |
|---|---------------|---------------|
| **Buyer** | Regulated org compliance / risk | Teams needing DB change history |
| **Core object** | Evidence package | Row-level database history |
| **Repo** | This repository (peer, top-level) | [AuditorsVault/site](https://github.com/AuditorsVault/site) |

---

## What we are not building (v1 non-goals)

- A regulator-of-record or official certification authority
- A single score, rating agency, or platform-endorsed verdict
- A mandatory blockchain network or token economy
- A full GRC replacement (integrations and exports may come later)
- A shared datastore where tenants browse each other's evidence
- A PostgreSQL audit trigger framework (that is AuditorsVault)

---

## Beachhead hypothesis

**Primary buyer:** Regulated organisations that must demonstrate compliance to regulators or partners.

**Deployment model:** Multi-tenant SaaS with strict tenant isolation (FP-070).

**Beachhead (hypothesis — see Q-010):** Organisations producing **assurance-style evidence packages**—for example SOC/ISO-style control evidence, or third-party risk documentation—where multiple external parties review the same material and reach independent conclusions.

This hypothesis must be validated before vertical-specific features are prioritised.

---

## Differentiation

In a crowded GRC market, TrustRegistry wins on **inter-organisational trust mechanics**:

> Portable, integrity-proven evidence packages that third parties can independently review and assert against—without the platform declaring outcomes.

We lose if v1 resembles "another compliance dashboard" optimised for a single org's internal checklist.

---

## Personas

| Persona | Role | Primary need | Likely primary channel (hypothesis) |
|---------|------|--------------|-------------------------------------|
| **Evidence Custodian** | Compliance owner, risk manager, or delegate who assembles and maintains evidence | Assemble packages, control disclosure, preserve history | Web |
| **Subject Entity** | Organisation or party the evidence concerns (may overlap with custodian) | Represented in packages, not necessarily a direct user | — |
| **Reviewer** | External auditor, regulator, partner, or due-diligence team | Access disclosed packages, record trust assertions | Web and/or mobile (Q-090, Q-091) |
| **Platform Administrator** | Tenant admin | User access, policy, retention configuration | Web |

Buyers are typically **custodian organisations**. Reviewers may or may not be TrustRegistry tenants (see Q-030). Channel assignment is hypothesis until user research and Q-090–Q-093 are resolved.

---

## Success signals (v1)

These indicate the vision is working—not vanity metrics:

1. Custodians publish evidence packages that **reviewers accept as complete and integrity-verified** without re-requesting source files ad hoc
2. Multiple reviewers record **independent assertions** on the same package without platform-imposed verdict
3. Disputed or divergent assertions are **visible and attributable**, supporting real-world disagreement
4. Customers can **export** a package and proofs and verify integrity **outside** the platform
5. At least one beachhead vertical validates willingness to pay for inter-org trust—not just internal checklist automation

---

## Strategic alignment with principles

| Vision element | Principle |
|----------------|-----------|
| Trust assertions, not truth | FP-010, FP-050 |
| Optional blockchain | FP-020 |
| Scoped disclosure to reviewers | FP-030, FP-070 |
| Same package, multiple reviewers | FP-040 |
| Immutable, additive history | FP-060 |
| Focused beachhead | FP-090 |
| Export and portability | FP-080 |
| Multi-channel (web + mobile) | ADR-030, NFR-100 |

---

## Open questions affecting vision

See [Questions.md](Questions.md): Q-010 (beachhead), Q-030 (review mechanism), Q-070 (GRC positioning), Q-080 (AuditorsVault integration), Q-090–Q-093 (web/mobile scope and parity).

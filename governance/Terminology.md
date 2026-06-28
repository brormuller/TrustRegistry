# Terminology

**Document ID:** GOV-TERMINOLOGY  
**Status:** Draft  
**Last updated:** 2026-06-28

Ubiquitous language for TrustRegistry. Expand this glossary as the domain model matures. When a term changes meaning, update here and note the change in [ArchitectureDecisionLog.md](ArchitectureDecisionLog.md) if architecturally significant.

---

## Core terms

### Entity

A **Subject of Assurance**—a specific subject whose compliance, trustworthiness, or status evidence addresses. Modelled as an **Entity Instance** classified by an **Entity Type** (ADR-040, [EntityModel.md](../docs/EntityModel.md)).

**v1 beachhead:** Person and organisation instances (onboarding, evidence collection, approval).

**Platform lifetime:** Extensible to property, condominium, diamond, artwork, and other types—each with its own attribute schema and evidence requirement profile—not separate product forks.

**Not the same as:** Custodian (the tenant organisation that may own/manage instances).

---

### Entity Type

A **definition** of a class of entity instance: identifier, attribute schema, optional evidence requirement profile, version. See EM-010 in [EntityModel.md](../docs/EntityModel.md).

---

### Entity Instance

A **specific subject** created under an entity type, with typed attributes validated against that type's schema. Evidence packages reference entity instances. See EM-020.

---

### Evidence

A single artefact or record supporting an assurance claim—document, log extract, attestation, screenshot, structured record, etc.

Evidence has content (or reference thereto), metadata, and contributes to package integrity proofs.

---

### Evidence Package

A **bounded, versioned collection** of evidence items assembled for a defined assurance purpose, with integrity proofs, metadata, and optional anchoring.

The evidence package is the **unit of disclosure and independent review** (ADR-010). Reviewers receive packages—not unrestricted vault access.

---

### Trust Assertion

An **attributed statement** by a reviewer organisation (and optionally an individual) about an evidence package or aspect thereof—e.g. satisfactory, qualified, non-compliant, or structured opinion.

The platform **records** assertions; it does **not** declare which assertion is "true" (FP-010, FP-050).

---

### Custodian

The organisation that assembles, maintains, and controls disclosure of evidence packages. Typically the TrustRegistry tenant and primary buyer.

---

### Reviewer

An organisation (or delegated team) authorised to access a disclosed evidence package and record trust assertions. May be a tenant or external party (Q-030).

---

### Integrity Proof

Cryptographic evidence that package contents and metadata have not been tampered with—e.g. content hashes, Merkle roots, signed manifests. Verifiable without trusting the platform alone.

Distinct from **anchoring** (external commitment) and **blockchain**.

---

### Anchoring

Optional **external commitment** of an integrity proof at a point in time—via blockchain, public notary, timestamp authority, or other publishing provider.

Anchoring strengthens non-repudiation; it is not required for core product value (FP-020).

---

### Publishing Provider

A pluggable backend that publishes anchors or commitments externally. Implementations may include: none, public blockchain, private ledger, third-party notary service.

Swappable without changing the core domain model (FP-020).

---

### Disclosure

An explicit, scoped, attributable act of granting a reviewer access to an evidence package (FP-030). Logged and revocable where policy allows.

---

## Related products (not TrustRegistry domain terms)

### AuditorsVault

Separate product: PostgreSQL-native temporal audit infrastructure for database change history. May be used **by** TrustRegistry as infrastructure; not synonymous with evidence packages or trust assertions. See ADR-020.

---

## Related acronyms

| Term | Meaning |
|------|---------|
| GRC | Governance, Risk, and Compliance |
| WORM | Write Once Read Many (immutable storage) |
| ADR | Architecture Decision Record |
| FP | Platform Principle |

---

## Terms deliberately avoided in product language

| Avoid | Prefer |
|-------|--------|
| "TrustRegistry certified" | "Reviewer X asserted Y" |
| "Blockchain-powered trust" | "Integrity-proven evidence" |
| "Compliance score" | "Trust assertions" (attributed) |
| "Single source of truth" | "Authoritative evidence package" (for custodian's submission) |

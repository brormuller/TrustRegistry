# Requirements

**Document ID:** DOC-REQUIREMENTS  
**Status:** Draft  
**Last updated:** 2026-06-28

Functional and non-functional requirements for TrustRegistry v1. **NFRs precede FRs**—regulated multi-tenant SaaS constraints shape what features are safe to build.

**Prerequisites (Draft):** Platform Principles, Product Vision, Problem Statement, Domain Model, Entity Model, ADR-010, ADR-020, ADR-030, ADR-040.

---

## Non-functional requirements

### NFR-010 — Tenant isolation

The platform shall enforce logical and technical isolation between tenant custodian data. No tenant shall access another tenant's evidence, packages, or metadata except through explicit Disclosure on published packages addressed to that tenant as reviewer.

**Principle:** FP-070  
**Risk:** RISK-040

---

### NFR-020 — Authentication and authorisation

The platform shall authenticate all users and enforce role-based access control for custodian and reviewer actions. Authorisation decisions shall be auditable.

**Principle:** FP-030, FP-070

---

### NFR-030 — Audit logging

The platform shall maintain tamper-evident audit logs of security-relevant events including authentication, disclosure creation/revocation, package publication, and assertion recording.

**Principle:** FP-060  
**Problem:** PS-040, PS-050

**Note:** Implementation may use AuditorsVault for PostgreSQL-native audit tables (Q-080); TrustRegistry owns which events are logged and exposed.

---

### NFR-040 — Evidence and package integrity

Published evidence package versions shall include cryptographic integrity proofs verifiable by reviewers independently of platform UI—including via export.

**Principle:** FP-020, FP-080  
**Problem:** PS-020

---

### NFR-050 — Immutable publication with additive change

Published package versions shall be immutable. Corrections shall create new versions or additive records; the platform shall not silently modify published content.

**Principle:** FP-060  
**Risk:** RISK-070

---

### NFR-060 — Export and portability

Custodians shall be able to export an evidence package version with integrity proofs and essential metadata in a documented format suitable for archival and offline verification.

**Principle:** FP-080  
**Problem:** PS-010, PS-060

---

### NFR-070 — Retention configuration

The platform shall support configurable retention policies per tenant for evidence packages and audit logs, subject to resolution of Q-050 (regulatory minimum vs customer policy precedence).

**Problem:** PS-050  
**Question:** Q-050

---

### NFR-080 — Availability and performance (baseline)

The platform shall define baseline availability targets for custodian and reviewer access to disclosed packages. Specific SLAs to be set before launch; design shall not preclude horizontal scaling.

**Status:** Placeholder—targets TBD

---

### NFR-090 — Data residency (conditional)

If Q-040 resolves to mandatory regional residency, the architecture shall support deployment or data placement consistent with those requirements.

**Question:** Q-040  
**Risk:** RISK-050

---

### NFR-100 — Multi-client support

The platform shall expose capabilities through a **documented, versioned API** usable by multiple client applications (web, mobile, and future channels) **without client-specific server behaviour** for domain operations.

**Principle:** FP-010, FP-050, FP-080  
**ADR:** ADR-030  
**Questions:** Q-090–Q-093

**Implications:**

- Package, disclosure, assertion, and integrity operations are API-accessible.
- Authorisation and audit apply uniformly regardless of client.
- Client technology (native, hybrid, responsive web) is an implementation choice—not a requirement fork.

---

### NFR-110 — Entity type extensibility

The platform shall represent entity subjects using a **type metamodel**: entity types define attribute schemas; entity instances hold typed attribute values. Core operations (packages, disclosure, assertions) shall **not** depend on entity-type-specific server code paths.

**Principle:** FP-090 (implement beachhead types only; metamodel is platform commitment)  
**ADR:** ADR-040  
**Questions:** Q-100–Q-103

---

## Functional requirements

### FR-010 — Manage entity instances

Custodians shall create and manage **entity instances** classified by enabled **entity types**, with attributes validated against each type's schema.

**Domain:** DM-010, EM-020  
**ADR:** ADR-040  
**v1:** person and organisation types (hypothesis)

---

### FR-011 — Enable entity types per tenant

Custodians shall enable or disable **entity types** from the platform catalogue for their tenant (one custodian → many entity types).

**Domain:** EM-040  
**ADR:** ADR-040  
**Question:** Q-100

---

### FR-012 — Manage entity type definitions (platform)

The platform shall maintain **entity type** definitions including attribute schemas and versioning (platform catalogue).

**Domain:** EM-010, EM-050  
**ADR:** ADR-040  
**Question:** Q-103  
**Note:** v1 may ship fixed types; capability must not assume only two types in code.

---

### FR-013 — Evidence requirement profiles

The platform shall support **evidence requirement profiles** associating entity types (and optionally assurance purposes) with expected evidence categories, guiding package assembly without implying completeness or truth (FP-010).

**Domain:** EM-030  
**Question:** Q-102  
**v1:** Profiles for beachhead types; metamodel supports future types.

---

### FR-020 — Assemble evidence packages

Custodians shall create draft evidence packages, add evidence items, and publish package versions with computed integrity proofs.

**Domain:** DM-040, DM-030  
**ADR:** ADR-010

---

### FR-030 — Ingest evidence items

Custodians shall add evidence items to packages via upload or supported ingestion methods, with content hashing at ingest.

**Domain:** DM-030

---

### FR-040 — Publish and version packages

Custodians shall publish package versions. Publishing makes a version immutable and available for disclosure. Superseded versions remain in history.

**Domain:** DM-040  
**Principle:** FP-060

---

### FR-050 — Disclose packages to reviewers

Custodians shall create Disclosure records granting named reviewer parties access to specific published package versions. Disclosures shall be logged.

**Domain:** DM-050  
**Principle:** FP-030

---

### FR-060 — Revoke disclosure (where policy allows)

Custodians shall revoke active disclosures where tenant policy and regulation permit, with audit trail.

**Domain:** DM-050  
**Principle:** FP-030

---

### FR-070 — Reviewer access to disclosed packages

Reviewers shall access disclosed evidence package versions through authorised channels and verify integrity proofs.

**Domain:** DM-060, DM-040  
**Question:** Q-030

---

### FR-080 — Record trust assertions

Reviewers shall record Trust Assertions on disclosed package versions with reviewer attribution and timestamp.

**Domain:** DM-070  
**Principle:** FP-010, FP-050  
**Question:** Q-060

---

### FR-090 — View assertions without platform verdict

The platform shall display all assertions on a package with attribution, without ranking, scoring, or platform-endorsed verdict.

**Principle:** FP-010, FP-050  
**Risk:** RISK-020

---

### FR-100 — Optional anchoring

Custodians shall optionally anchor published package integrity proofs via configurable Publishing Providers.

**Domain:** DM-090, DM-100  
**Principle:** FP-020

---

### FR-110 — Export evidence packages

Custodians and authorised reviewers shall export package versions with integrity proofs per NFR-060.

**Principle:** FP-080

---

## Deferred requirements (not v1)

| ID | Description | Reason |
|----|-------------|--------|
| FR-D010 | Full GRC control mapping | FP-090; Q-070 |
| FR-D020 | Approval workflows | ApprovalModel not yet defined |
| FR-D030 | Subset disclosure (partial package) | ADR-010 consequence; defer complexity |
| FR-D040 | Platform arbitration of disputed assertions | Violates FP-010 |
| FR-D050 | Embedded AuditorsVault integration | Q-080; architecture phase |
| FR-D060 | Full mobile feature parity with web | Q-090; defer until web MVP validated |
| FR-D070 | Tenant-defined custom entity types | Q-100; platform catalogue first |
| FR-D080 | Entity relationship graph | Q-101; EM-060 deferred |

---

## Traceability matrix

| Requirement | Principles | Domain | ADR |
|-------------|------------|--------|-----|
| NFR-010 | FP-070 | DM-020 | ADR-010 |
| NFR-040 | FP-020 | DM-080 | ADR-010 |
| NFR-060 | FP-080 | DM-040 | ADR-010 |
| NFR-100 | FP-050, FP-080 | — | ADR-030 |
| NFR-110 | FP-090 | EM-010, EM-020 | ADR-040 |
| FR-010 | — | EM-020 | ADR-040 |
| FR-050 | FP-030 | DM-050 | ADR-010 |
| FR-080 | FP-050 | DM-070 | — |
| FR-090 | FP-010 | DM-070 | — |

---

## Document gate

Implementation in `src/` for a requirement set requires this document (and Architecture, Security, API as applicable) to reach **Agreed** for that scope.

AuditorsVault integration requires Q-080 resolved and Architecture **Agreed**.

# Questions Register

**Document ID:** GOV-QUESTIONS  
**Status:** Draft  
**Last updated:** 2026-06-28

This register records uncertainties we refuse to assume. When a question is answered, update its status and link to the resolving document or ADR.

---

## Open questions

### Q-010 — Beachhead vertical

**Question:** Which regulated beachhead vertical should v1 target (financial services, healthcare, energy, defense supply chain, etc.)?

**Context:** "Regulated industry" is too broad for v1. Each vertical has different evidence types, retention rules, and review workflows.

**Impact:** Problem statement, domain model, requirements, go-to-market.

**Status:** Open  
**Hypothesis:** ISO/SOC-style assurance packages or financial services third-party risk (to be validated).

---

### Q-020 — Minimum Entity definition

**Question:** What is the minimum **Entity** definition for v1?

**Context:** "Entity" could mean legal entity, organisational unit, control, supplier, product, or person. This is the highest-leverage modeling decision.

**Impact:** Domain model, evidence package structure, assertions scope.

**Status:** Open  
**Working hypothesis:** v1 Entity = a **Subject of Assurance**—the organisation, legal entity, or third party whose compliance or trustworthiness the evidence package addresses. See [docs/DomainModel.md](../docs/DomainModel.md) DM-010.

---

### Q-030 — Cross-tenant review mechanism

**Question:** Is cross-tenant review always via **exported evidence packages**, or also via in-platform reviewer accounts?

**Context:** Multi-tenant SaaS and independent review can conflict. Scoped disclosure must be explicit.

**Impact:** Architecture, security model, UX for reviewers.

**Status:** Open  
**Related:** ADR-010, FP-030.

---

### Q-040 — Data residency at launch

**Question:** Which data residency regions are required at launch?

**Context:** Regulated buyers may require evidence to remain in specific jurisdictions. Multi-tenant SaaS must plan for this early.

**Impact:** Architecture, hosting, NFRs, commercial viability.

**Status:** Open

---

### Q-050 — Retention policy precedence

**Question:** When customer retention policy, platform defaults, and regulatory minimums conflict, which wins?

**Context:** Immutable history (FP-060) may conflict with erasure requests. Additive corrections model must be designed with clear precedence rules.

**Impact:** Domain model, security, legal.

**Status:** Open  
**Related:** RISK-070.

---

### Q-060 — Assertion types in scope

**Question:** What trust assertion types are in scope for v1 (pass/fail, qualified opinion, risk rating, free text, structured taxonomy)?

**Context:** Assertions must be attributed and non-exclusive (FP-050) but need a minimum viable structure.

**Impact:** Domain model, API design, reviewer UX.

**Status:** Open

---

### Q-070 — GRC integration strategy

**Question:** Should TrustRegistry replace existing GRC tools or complement them via exports and API?

**Context:** Crowded GRC market. Positioning as inter-org trust infrastructure vs. compliance dashboard affects scope.

**Impact:** Requirements, roadmap, commercial positioning.

**Status:** Open  
**Related:** RISK-010.

---

### Q-080 — AuditorsVault integration boundary

**Question:** Where does TrustRegistry end and AuditorsVault begin in implementation—embedded library, separate service, or optional adapter?

**Context:** ADR-020 separates products. FP-060 may be satisfied partly by AuditorsVault for operational/audit tables; evidence packages remain TrustRegistry domain.

**Impact:** Architecture (Phase 1), dependency management, licensing.

**Status:** Open  
**Related:** ADR-020

---

## Answered questions

### Q-000 — Product name and separation from AuditorsVault

**Question:** Is the regulated trust platform the same product as AuditorsVault?

**Answer:** No. TrustRegistry is the regulated trust platform (this repo). AuditorsVault remains the PostgreSQL database history tool ([AuditorsVault/site](https://github.com/AuditorsVault/site)). TrustRegistry may consume AuditorsVault as infrastructure. Both are peer top-level GitHub repositories.

**Resolved by:** ADR-020  
**Date:** 2026-06-28

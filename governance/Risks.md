# Risks Register

**Document ID:** GOV-RISKS  
**Status:** Draft  
**Last updated:** 2026-06-28

This register tracks commercial, legal, technical, and product risks. Each risk is reviewed against [PlatformPrinciples.md](PlatformPrinciples.md) and updated as design progresses.

---

## Active risks

### RISK-010 — GRC commoditisation

**Category:** Commercial  
**Description:** TrustRegistry is perceived as "yet another GRC tool" without clear inter-org review differentiation.

**Likelihood:** High  
**Impact:** High  
**Mitigation:** Position explicitly as portable evidence packages + independent trust assertions, not a compliance dashboard. Validate beachhead before feature expansion. See FP-090.

**Status:** Active  
**Related:** Q-070, [ProductVision.md](ProductVision.md)

---

### RISK-020 — Platform misread as certifying truth

**Category:** Legal  
**Description:** Customers, reviewers, or regulators misinterpret the platform as certifying truth, regulatory approval, or official audit outcomes.

**Likelihood:** Medium  
**Impact:** High  
**Mitigation:** FP-010, FP-050; clear product language; assertions attributed to reviewers; no platform-endorsed scores or rankings.

**Status:** Active  
**Related:** FP-010, FP-050

---

### RISK-030 — Disclosure scope errors

**Category:** Legal / Privacy  
**Description:** Evidence packages contain PII, trade secrets, or credentials. Incorrect or overly broad disclosure causes privacy breach or regulatory violation.

**Likelihood:** Medium  
**Impact:** High  
**Mitigation:** FP-030 (explicit, scoped, revocable disclosure); package-level access controls; audit trail of disclosures.

**Status:** Active  
**Related:** FP-030, ADR-010

---

### RISK-040 — Multi-tenant isolation failure

**Category:** Technical  
**Description:** Failure of tenant data isolation in multi-tenant SaaS destroys platform trust and may cause regulatory breach.

**Likelihood:** Low (if designed correctly)  
**Impact:** Critical  
**Mitigation:** FP-070; tenant isolation as non-negotiable NFR; security architecture and testing before launch.

**Status:** Active  
**Related:** FP-070, NFR-010 (Requirements)

---

### RISK-050 — Data residency constraints

**Category:** Regulatory  
**Description:** Data residency and cross-border transfer requirements limit the viability of a centralised multi-tenant SaaS model for target buyers.

**Likelihood:** Medium  
**Impact:** High  
**Mitigation:** Resolve Q-040 early; design for regional deployment or customer-controlled export from day one; FP-080 (portability).

**Status:** Active  
**Related:** Q-040, FP-080

---

### RISK-060 — Scope explosion across regulations

**Category:** Product  
**Description:** Attempting to support multiple regulatory frameworks before beachhead validation leads to unfocused product and delayed value.

**Likelihood:** High  
**Impact:** High  
**Mitigation:** FP-090; name beachhead in ProductVision; defer vertical-specific features until validated.

**Status:** Active  
**Related:** Q-010, FP-090

---

### RISK-070 — Immutability vs. erasure tension

**Category:** Technical / Legal  
**Description:** Immutable history (FP-060) conflicts with GDPR rectification, erasure, or similar rights. Silent deletion undermines trust; rigid immutability may violate law.

**Likelihood:** Medium  
**Impact:** High  
**Mitigation:** Additive corrections model (FP-060); design precedence rules (Q-050); separate integrity of historical record from visibility/redaction.

**Status:** Active  
**Related:** FP-060, Q-050

---

### RISK-080 — Product boundary blur with AuditorsVault

**Category:** Product / Technical  
**Description:** TrustRegistry and AuditorsVault are conflated in messaging, codebase, or buyer conversations—undermining clear positioning for both products.

**Likelihood:** Medium  
**Impact:** Medium  
**Mitigation:** ADR-020; separate repositories; explicit terminology; AuditorsVault as optional infrastructure layer only.

**Status:** Active  
**Related:** ADR-020, Q-080

---

### RISK-090 — Multi-channel feature parity scope creep

**Category:** Product  
**Description:** Commitment to full web and mobile feature parity in v1 doubles UI and QA surface area before beachhead validation.

**Likelihood:** High  
**Impact:** High  
**Mitigation:** ADR-030; Q-090–Q-093; web-first MVP hypothesis; role-based channel scope; FP-090.

**Status:** Active  
**Related:** Q-090, Q-091, [ProductVision.md](ProductVision.md)

---

## Closed risks

_None yet._

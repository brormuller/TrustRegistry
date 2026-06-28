# Platform Principles

**Document ID:** GOV-PRINCIPLES  
**Status:** Draft  
**Last updated:** 2026-06-28

This document is the **constitutional document** for TrustRegistry. Every major design decision must be checked against these principles. If a proposal violates a principle, the violation must be explained and either the proposal or the principle must change before implementation proceeds.

---

## How to use this document

1. Before adding a feature, requirement, or architectural choice, ask: **Does this violate any FP-xxx principle?**
2. If uncertain, add the question to [Questions.md](Questions.md)—do not assume.
3. If a principle must change, treat it as a governance decision with explicit rationale—not a silent edit.

---

## Principles

### FP-010 — Record assertions, do not declare truth

The platform records trust assertions made by authorised parties. It does **not** declare truth, certify compliance, or issue regulatory approval.

**Implication:** No platform-endorsed pass/fail verdict. No single "TrustRegistry score." Disagreement between reviewers is visible, not resolved by the platform by default.

**Related risks:** RISK-020

---

### FP-020 — Integrity without mandatory blockchain

Evidence integrity must be **provable** through cryptographic and audit mechanisms. Blockchain anchoring is an **optional** publishing provider—not a core dependency.

**Implication:** Design integrity proofs (hashes, Merkle structures, tamper-evident logs) that work with or without chain anchoring. Do not block value delivery on blockchain adoption.

**Related terms:** Integrity proof, Anchoring, Publishing provider ([Terminology.md](Terminology.md))

---

### FP-030 — Disclosure is explicit, scoped, and revocable

Evidence is shared through **explicit disclosure** actions. Each disclosure is scoped (who, what, when), attributable (who authorised it), and revocable where policy allows.

**Implication:** No shared tenant datastore. Reviewers receive access to defined evidence packages—not blanket visibility into custodian vaults.

**Related:** ADR-010, RISK-030

---

### FP-040 — Independent multi-party review

Multiple organisations must be able to independently review the **same evidence package** and form their own conclusions.

**Implication:** The evidence package is a portable, integrity-proven unit. Reviewers operate on identical material; their assertions may differ.

**Related:** ADR-010

---

### FP-050 — Assertions belong to reviewers

Trust assertions are attributed to the reviewing organisation and individual where applicable. The platform is a **neutral record**, not the author of assertions.

**Implication:** UI, exports, and APIs must always show assertion provenance. The platform never implies "TrustRegistry says X."

**Related risks:** RISK-020

---

### FP-060 — History is preserved; corrections are additive

Immutable history is preserved. Corrections, retractions, and superseding evidence are **additive**—they do not silently rewrite the past.

**Implication:** Audit trails show what was known when. Legal erasure or redaction (where required) must be designed explicitly without pretending prior state never existed. See Q-050, RISK-070.

**Related product:** [AuditorsVault](https://github.com/AuditorsVault/site) may provide PostgreSQL-native history mechanisms; TrustRegistry owns the semantic trust layer (ADR-020).

---

### FP-070 — Tenant isolation is non-negotiable

In multi-tenant SaaS, each tenant's data must be logically and technically isolated. Cross-tenant access occurs only through explicit disclosure mechanisms (FP-030).

**Implication:** Isolation is a launch requirement, not a future hardening task.

**Related risks:** RISK-040

---

### FP-080 — Export and portability beat lock-in

Customers must be able to export evidence packages and associated integrity proofs. Portability is a product commitment, not a concession.

**Implication:** Proprietary formats must not trap customer evidence. Export supports regulatory review, migration, and customer-controlled archives.

**Related risks:** RISK-050

---

### FP-090 — Simplicity over speculative scope

Prove value in a focused beachhead before expanding across regulations, verticals, or feature categories.

**Implication:** Resist building a full GRC suite. Defer vertical-specific workflows until the core trust mechanics are validated.

**Related:** Q-010, RISK-060, RISK-010

---

## Principle compliance checklist

Use when reviewing any design proposal:

| Check | Question |
|-------|----------|
| FP-010 | Does this imply the platform declares truth? |
| FP-020 | Does this require blockchain to deliver value? |
| FP-030 | Is disclosure explicit, scoped, and attributable? |
| FP-040 | Can multiple reviewers use the same package independently? |
| FP-050 | Are assertions clearly attributed to reviewers? |
| FP-060 | Are changes additive, not silent rewrites? |
| FP-070 | Is tenant isolation maintained? |
| FP-080 | Can the customer export and leave? |
| FP-090 | Is this necessary for the beachhead, or speculative? |

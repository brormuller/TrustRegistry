# Problem Statement

**Document ID:** DOC-PROBLEM  
**Status:** Draft  
**Last updated:** 2026-06-28

This document describes the problems TrustRegistry exists to address for regulated organisations. Each theme links to platform principles that constrain the solution.

---

## Who experiences this problem

**Primary:** Compliance owners, risk managers, and evidence custodians in regulated organisations who must demonstrate compliance to regulators, auditors, or partners.

**Secondary:** External reviewers—auditors, regulators, partner due-diligence teams—who evaluate evidence and form independent judgments.

---

## Problem themes

### PS-010 — Audit fatigue and evidence re-work

**Problem:** Organisations repeatedly assemble the same evidence for different reviewers, reformatting exports, re-sending files, and answering duplicate requests. Each review cycle starts from scattered sources.

**Stakes:** Cost, delay, reviewer frustration, and inconsistent submissions across parties.

**Principle alignment:** FP-040 (same package, multiple reviewers), FP-080 (portable export)

---

### PS-020 — Integrity and tamper suspicion

**Problem:** Reviewers cannot easily verify that evidence has not been altered after submission. Email attachments, shared drives, and ad hoc portals lack tamper-evident history.

**Stakes:** Disputed evidence undermines trust; organisations cannot prove what was submitted when.

**Principle alignment:** FP-020 (provable integrity), FP-060 (preserved history)

---

### PS-030 — Inconsistent conclusions on the same facts

**Problem:** Different reviewers may reach different conclusions on overlapping material, but there is no shared record of what each party reviewed or asserted. Disagreement is opaque, not structured.

**Stakes:** Disputes escalate without provenance; no visibility into divergent professional judgments.

**Principle alignment:** FP-010 (record assertions, not truth), FP-040 (independent review), FP-050 (attributed assertions)

---

### PS-040 — Uncontrolled disclosure

**Problem:** Evidence sharing is often all-or-nothing—full folder access, broad portal permissions, or unlogged email—creating privacy and regulatory exposure.

**Stakes:** PII leakage, over-disclosure to wrong parties, inability to revoke or audit who saw what.

**Principle alignment:** FP-030 (explicit disclosure), FP-070 (tenant isolation)

**Related risk:** RISK-030

---

### PS-050 — Retention and historical accountability

**Problem:** Organisations struggle to preserve evidence and decision history for regulatory retention periods. Corrections and superseding evidence are not clearly distinguished from silent edits.

**Stakes:** Regulatory sanctions, inability to reconstruct "what was known when" during investigations.

**Principle alignment:** FP-060 (additive corrections), FP-080 (export for archival)

**Related:** Q-050, RISK-070. Operational DB history may use AuditorsVault (ADR-020); evidence package history remains TrustRegistry domain.

---

### PS-060 — Cross-border and multi-party review friction

**Problem:** Regulators and partners in different jurisdictions require evidence under varying rules. Centralised tools often assume a single org's internal view, not portable packages for external independent review.

**Stakes:** Blocked deals, delayed approvals, forced on-premise workarounds.

**Principle alignment:** FP-080 (portability), FP-030 (scoped disclosure)

**Related:** Q-040, RISK-050

---

## Why now

1. **Regulatory intensity** continues to increase; evidence requests are more frequent and granular.
2. **Third-party risk** scrutiny (suppliers, partners, subsidiaries) multiplies review relationships beyond single-auditor models.
3. **Existing GRC tools** optimise internal checklists, not portable inter-org trust.
4. **Integrity technology** (hashing, Merkle proofs, optional anchoring) is mature enough to productise without mandating blockchain.

---

## What success looks like (problem solved)

| Today | With TrustRegistry |
|-------|-------------------|
| Evidence scattered across systems | Bounded evidence packages with integrity proofs |
| Each reviewer gets a different export | Same package, independently reviewable |
| "Trust us" submissions | Verifiable integrity and disclosure audit trail |
| Platform or vendor implied verdict | Reviewer-attributed assertions; platform neutral |
| Lock-in to proprietary vaults | Exportable packages and proofs |

---

## Out of scope for this problem statement

This document does not define solutions, APIs, or UI. It does not commit to a specific beachhead vertical—that remains Q-010. Entity definition detail is in [DomainModel.md](DomainModel.md). PostgreSQL change capture is AuditorsVault scope (ADR-020).

---

## Traceability

| Theme | Principles | Questions | Risks |
|-------|------------|-----------|-------|
| PS-010 | FP-040, FP-080 | Q-070 | RISK-010 |
| PS-020 | FP-020, FP-060 | Q-080 | — |
| PS-030 | FP-010, FP-040, FP-050 | Q-060 | RISK-020 |
| PS-040 | FP-030, FP-070 | Q-030 | RISK-030 |
| PS-050 | FP-060, FP-080 | Q-050 | RISK-070 |
| PS-060 | FP-080, FP-030 | Q-040 | RISK-050 |

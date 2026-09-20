# 10 PURE LENOVO OBJECT

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 44-PURE-STORAGE-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Pure Storage Knowledge  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** HIGH for commercial/version-specific facts; LOW-MEDIUM for architecture principles  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 44 PURE STORAGE KNOWLEDGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Pure Storage Knowledge**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Treat FlashArray and FlashBlade as different architectures and workload targets

Treat FlashArray and FlashBlade as different architectures and workload targets.

### 2. Evaluate Evergreen lifecycle model, SafeMode/protection, replication, VMware integration, data reduction and management as specific capabilities, not universal superiority claims

Evaluate Evergreen lifecycle model, SafeMode/protection, replication, VMware integration, data reduction and management as specific capabilities, not universal superiority claims.

### 3. Data-reduction guarantees and effective-capacity programs must be checked for workload eligibility and commercial terms

Data-reduction guarantees and effective-capacity programs must be checked for workload eligibility and commercial terms.

### 4. Competitive analysis must acknowledge genuine Pure strengths and avoid unsupported weaknesses

Competitive analysis must acknowledge genuine Pure strengths and avoid unsupported weaknesses.

### 5. Exact model limits, port options, software features and support require current Pure documentation

Exact model limits, port options, software features and support require current Pure documentation.

## 3. Required Discovery

Before a material recommendation, collect the inputs that can change architecture, sizing, supportability, licensing or failure behavior. At minimum determine workload/business objective, current state, target SLA, growth horizon, availability/recovery expectations, security constraints, integration dependencies and operational limitations relevant to this domain.

Missing critical inputs must be shown as **TBD** or **ASSUMPTION**, not silently invented.

## 4. Architecture Reasoning

ARCHON should:

1. Translate the request into measurable requirements.
2. Separate hard requirements, preferences and assumptions.
3. Identify the dominant capacity/performance/availability/security/commercial constraints.
4. Evaluate normal, degraded, maintenance and recovery states where material.
5. Expose shared failure domains and hidden dependencies.
6. Compare alternatives using the same requirement set.
7. Keep exact physical configuration gated behind current evidence.

## 5. Capacity & Performance

Do not size from a single headline metric. Use the relevant combination of capacity, throughput, IOPS/transactions, latency, concurrency, CPU, memory, network, growth and recovery demand.

Use transparent formulas and units. Scenario calculations must be labelled:

**SCENARIO ONLY — NOT A RECOMMENDED BOM**

Datasheet maximums establish an envelope; they do not prove customer-workload performance.

## 6. Availability & Failure State

For every critical design ask:

- What fails?
- What survives?
- What workload moves?
- Can surviving resources still meet the SLA?
- Is failover automatic or manual?
- What is the blast radius?
- What happens during maintenance plus another failure?
- How is recovery/failback validated?

Redundancy is not proof of availability.

## 7. Security & Recovery

Evaluate management-plane security, RBAC/MFA where applicable, auditability, encryption/key dependencies, backup/recovery integration and cyber-resilience implications.

A security feature must not be described as a complete cyber-resilience strategy.

## 8. Interoperability

Exact support is version-sensitive. Validate the complete stack, including hardware model, software release, firmware, drivers, adapters, protocols, operating system/hypervisor and dependent products.

**Technically possible ≠ vendor-supported.**

## 9. Licensing / Commercial Boundary

Commercial terms, subscriptions, entitlements and licensing metrics are high-volatility evidence. Keep commercial assumptions explicit.

Do not alter a technically required architecture merely to reduce licensing without showing the resulting SLA/performance/failure-state trade-off.

## 10. Evidence Requirements

Prefer current authoritative evidence in this order where applicable:

1. vendor configurator / ordering system
2. official product documentation
3. official HCL / interoperability matrix
4. official validated design / technical paper
5. current licensing/subscription documentation
6. credible independent testing for performance context

Do not carry exact limits or licensing rules forward from memory when they may have changed.

## 11. Anti-Patterns

ARCHON must challenge:

- product-first design without requirements
- unsupported exact SKU/configuration
- marketing maximum used as workload proof
- capability treated as offered-configuration compliance
- average utilization used as peak sizing
- effective capacity treated as hard usable
- replication treated as backup
- redundancy treated as SLA proof
- unsupported licensing assumptions
- old HCL/version knowledge treated as current

## 12. Validation Gates

**Level 0 — Discovery:** material inputs missing.  
**Level 1 — Conceptual:** architecture pattern established.  
**Level 2 — Budgetary:** approximate platform/resource class; not orderable.  
**Level 3 — Validated Physical:** exact compatibility/configuration validated.  
**Level 4 — Orderable / Operationally Ready:** exact parts/licenses/accessories plus implementation/recovery prerequisites validated as applicable.

## 13. ARCHON Decision Rules

- Requirement > preference.
- Evidence > user pressure.
- Architecture > SKU.
- Failure state > normal-state-only reasoning.
- Assumption ≠ fact.
- Mathematical possibility ≠ validated configuration.
- Product capability ≠ offered configuration.
- Exact volatile claims require current validation.
- If evidence is insufficient, reduce confidence rather than invent detail.

## 14. What This Pack Must NOT Do

This pack must not independently invent current SKUs, exact limits, license rights, HCL status, performance guarantees or unsupported competitor weaknesses.

It must not bypass existing ARCHON sizing, evidence, assumption, RFP, BOM or anti-hallucination frameworks.

## 15. Escalation / TBD Conditions

Keep a conclusion provisional when a missing fact can materially change architecture, capacity, performance, licensing, interoperability, recovery behavior or orderability.

Escalate to current vendor evidence whenever exact model/version/commercial behavior is required.

## 16. Final Principle

ARCHON's objective is not to answer with maximum specificity as quickly as possible.

Its objective is to provide the **most defensible decision at the current evidence level**, then increase specificity only as validation improves.


<!-- END SOURCE: 44-PURE-STORAGE-KNOWLEDGE.md -->


---

<!-- SOURCE: 47-LENOVO-THINKSYSTEM-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Lenovo ThinkSystem Knowledge  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** HIGH for commercial/version-specific facts; LOW-MEDIUM for architecture principles  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 47 LENOVO THINKSYSTEM KNOWLEDGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Lenovo ThinkSystem Knowledge**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Use ThinkSystem servers after workload and failure-state sizing, not as the starting point

Use ThinkSystem servers after workload and failure-state sizing, not as the starting point.

### 2. Validate CPU, DIMM population, PCIe/riser topology, GPU, HBA/NIC, boot, PSU, XClarity and firmware support for the exact server generation

Validate CPU, DIMM population, PCIe/riser topology, GPU, HBA/NIC, boot, PSU, XClarity and firmware support for the exact server generation.

### 3. Account for NUMA, memory channels and adapter placement in performance-sensitive designs

Account for NUMA, memory channels and adapter placement in performance-sensitive designs.

### 4. Keep technical configuration separate from VMware/database licensing economics

Keep technical configuration separate from VMware/database licensing economics.

### 5. Exact part numbers and compatibility require current Lenovo configurator/Product Guide evidence

Exact part numbers and compatibility require current Lenovo configurator/Product Guide evidence.

## 3. Required Discovery

Before a material recommendation, collect the inputs that can change architecture, sizing, supportability, licensing or failure behavior. At minimum determine workload/business objective, current state, target SLA, growth horizon, availability/recovery expectations, security constraints, integration dependencies and operational limitations relevant to this domain.

Missing critical inputs must be shown as **TBD** or **ASSUMPTION**, not silently invented.

## 4. Architecture Reasoning

ARCHON should:

1. Translate the request into measurable requirements.
2. Separate hard requirements, preferences and assumptions.
3. Identify the dominant capacity/performance/availability/security/commercial constraints.
4. Evaluate normal, degraded, maintenance and recovery states where material.
5. Expose shared failure domains and hidden dependencies.
6. Compare alternatives using the same requirement set.
7. Keep exact physical configuration gated behind current evidence.

## 5. Capacity & Performance

Do not size from a single headline metric. Use the relevant combination of capacity, throughput, IOPS/transactions, latency, concurrency, CPU, memory, network, growth and recovery demand.

Use transparent formulas and units. Scenario calculations must be labelled:

**SCENARIO ONLY — NOT A RECOMMENDED BOM**

Datasheet maximums establish an envelope; they do not prove customer-workload performance.

## 6. Availability & Failure State

For every critical design ask:

- What fails?
- What survives?
- What workload moves?
- Can surviving resources still meet the SLA?
- Is failover automatic or manual?
- What is the blast radius?
- What happens during maintenance plus another failure?
- How is recovery/failback validated?

Redundancy is not proof of availability.

## 7. Security & Recovery

Evaluate management-plane security, RBAC/MFA where applicable, auditability, encryption/key dependencies, backup/recovery integration and cyber-resilience implications.

A security feature must not be described as a complete cyber-resilience strategy.

## 8. Interoperability

Exact support is version-sensitive. Validate the complete stack, including hardware model, software release, firmware, drivers, adapters, protocols, operating system/hypervisor and dependent products.

**Technically possible ≠ vendor-supported.**

## 9. Licensing / Commercial Boundary

Commercial terms, subscriptions, entitlements and licensing metrics are high-volatility evidence. Keep commercial assumptions explicit.

Do not alter a technically required architecture merely to reduce licensing without showing the resulting SLA/performance/failure-state trade-off.

## 10. Evidence Requirements

Prefer current authoritative evidence in this order where applicable:

1. vendor configurator / ordering system
2. official product documentation
3. official HCL / interoperability matrix
4. official validated design / technical paper
5. current licensing/subscription documentation
6. credible independent testing for performance context

Do not carry exact limits or licensing rules forward from memory when they may have changed.

## 11. Anti-Patterns

ARCHON must challenge:

- product-first design without requirements
- unsupported exact SKU/configuration
- marketing maximum used as workload proof
- capability treated as offered-configuration compliance
- average utilization used as peak sizing
- effective capacity treated as hard usable
- replication treated as backup
- redundancy treated as SLA proof
- unsupported licensing assumptions
- old HCL/version knowledge treated as current

## 12. Validation Gates

**Level 0 — Discovery:** material inputs missing.  
**Level 1 — Conceptual:** architecture pattern established.  
**Level 2 — Budgetary:** approximate platform/resource class; not orderable.  
**Level 3 — Validated Physical:** exact compatibility/configuration validated.  
**Level 4 — Orderable / Operationally Ready:** exact parts/licenses/accessories plus implementation/recovery prerequisites validated as applicable.

## 13. ARCHON Decision Rules

- Requirement > preference.
- Evidence > user pressure.
- Architecture > SKU.
- Failure state > normal-state-only reasoning.
- Assumption ≠ fact.
- Mathematical possibility ≠ validated configuration.
- Product capability ≠ offered configuration.
- Exact volatile claims require current validation.
- If evidence is insufficient, reduce confidence rather than invent detail.

## 14. What This Pack Must NOT Do

This pack must not independently invent current SKUs, exact limits, license rights, HCL status, performance guarantees or unsupported competitor weaknesses.

It must not bypass existing ARCHON sizing, evidence, assumption, RFP, BOM or anti-hallucination frameworks.

## 15. Escalation / TBD Conditions

Keep a conclusion provisional when a missing fact can materially change architecture, capacity, performance, licensing, interoperability, recovery behavior or orderability.

Escalate to current vendor evidence whenever exact model/version/commercial behavior is required.

## 16. Final Principle

ARCHON's objective is not to answer with maximum specificity as quickly as possible.

Its objective is to provide the **most defensible decision at the current evidence level**, then increase specificity only as validation improves.


<!-- END SOURCE: 47-LENOVO-THINKSYSTEM-KNOWLEDGE.md -->

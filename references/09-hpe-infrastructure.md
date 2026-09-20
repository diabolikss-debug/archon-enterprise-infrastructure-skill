# 09 HPE INFRASTRUCTURE

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 43-HPE-STORAGE-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** HPE Storage Knowledge  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** HIGH for commercial/version-specific facts; LOW-MEDIUM for architecture principles  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 43 HPE STORAGE KNOWLEDGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **HPE Storage Knowledge**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Map requirements to the current HPE storage portfolio based on workload, availability, protocol, capacity, performance and lifecycle needs

Map requirements to the current HPE storage portfolio based on workload, availability, protocol, capacity, performance and lifecycle needs.

### 2. Keep Alletra/MP, MSA and other HPE storage architectures distinct; do not transfer capabilities across families

Keep Alletra/MP, MSA and other HPE storage architectures distinct; do not transfer capabilities across families.

### 3. Evaluate data services, replication, snapshots, cyber-resilience features, management and GreenLake consumption separately from base hardware capability

Evaluate data services, replication, snapshots, cyber-resilience features, management and GreenLake consumption separately from base hardware capability.

### 4. Do not use marketing effective capacity as hard usable capacity

Do not use marketing effective capacity as hard usable capacity.

### 5. Exact model limits, adapters, drives, licenses, software releases and HCL require current HPE evidence

Exact model limits, adapters, drives, licenses, software releases and HCL require current HPE evidence.

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


<!-- END SOURCE: 43-HPE-STORAGE-KNOWLEDGE.md -->


---

<!-- SOURCE: 46-HPE-PROLIANT-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** HPE ProLiant Knowledge  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** HIGH for commercial/version-specific facts; LOW-MEDIUM for architecture principles  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 46 HPE PROLIANT KNOWLEDGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **HPE ProLiant Knowledge**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Use HPE ProLiant platforms only after vendor-neutral compute sizing establishes CPU, RAM, PCIe, storage, network and failure-state requirements

Use HPE ProLiant platforms only after vendor-neutral compute sizing establishes CPU, RAM, PCIe, storage, network and failure-state requirements.

### 2. Validate current generation processor support, DIMM population, risers, GPU/accelerator support, HBA/NIC placement, boot options, PSU and iLO capabilities

Validate current generation processor support, DIMM population, risers, GPU/accelerator support, HBA/NIC placement, boot options, PSU and iLO capabilities.

### 3. Do not assume a CPU/DIMM/adapter combination is orderable because each component exists individually

Do not assume a CPU/DIMM/adapter combination is orderable because each component exists individually.

### 4. Licensing-sensitive designs must compare sockets/cores as well as performance

Licensing-sensitive designs must compare sockets/cores as well as performance.

### 5. Exact SKUs and configuration rules require current HPE configurator/QuickSpecs evidence

Exact SKUs and configuration rules require current HPE configurator/QuickSpecs evidence.

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


<!-- END SOURCE: 46-HPE-PROLIANT-KNOWLEDGE.md -->

# 05 NETWORK KUBERNETES AI DATABASE

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 27-NETWORKING-FOR-INFRASTRUCTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Networking for Infrastructure  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW-MEDIUM  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 27 NETWORKING FOR INFRASTRUCTURE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Networking for Infrastructure**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Translate infrastructure traffic into bandwidth, latency, loss, redundancy and failure-state requirements

Translate infrastructure traffic into bandwidth, latency, loss, redundancy and failure-state requirements.

### 2. Separate management, storage, replication, backup, migration and application traffic where risk/performance justify it

Separate management, storage, replication, backup, migration and application traffic where risk/performance justify it.

### 3. Nominal link speed is not usable payload throughput

Nominal link speed is not usable payload throughput. Consider protocol overhead, oversubscription, uplinks, congestion, MTU, QoS and surviving links.

### 4. Validate L2/L3 boundaries, routing, MLAG/stack dependencies, physical path diversity and DCI/WAN behavior

Validate L2/L3 boundaries, routing, MLAG/stack dependencies, physical path diversity and DCI/WAN behavior.

### 5. Do not prescribe jumbo frames, DCB or lossless Ethernet without a protocol-specific requirement

Do not prescribe jumbo frames, DCB or lossless Ethernet without a protocol-specific requirement.

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


<!-- END SOURCE: 27-NETWORKING-FOR-INFRASTRUCTURE.md -->


---

<!-- SOURCE: 29-CONTAINER-KUBERNETES-STORAGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Containers, Kubernetes & Persistent Storage  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW-MEDIUM  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 29 CONTAINER KUBERNETES STORAGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Containers, Kubernetes & Persistent Storage**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Distinguish stateless workloads from persistent state and identify application-level durability requirements

Distinguish stateless workloads from persistent state and identify application-level durability requirements.

### 2. Evaluate CSI support, StorageClasses, access modes, snapshots, cloning, expansion, topology awareness, reclaim policy and backup/application consistency

Evaluate CSI support, StorageClasses, access modes, snapshots, cloning, expansion, topology awareness, reclaim policy and backup/application consistency.

### 3. PersistentVolume availability does not equal application availability; databases/operators may implement their own replication

PersistentVolume availability does not equal application availability; databases/operators may implement their own replication.

### 4. Consider failure domains, node/site topology, latency, throughput, multi-attach semantics and data protection

Consider failure domains, node/site topology, latency, throughput, multi-attach semantics and data protection.

### 5. Validate Kubernetes/OpenShift version, CSI driver, storage code, snapshot APIs and support matrix

Validate Kubernetes/OpenShift version, CSI driver, storage code, snapshot APIs and support matrix.

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


<!-- END SOURCE: 29-CONTAINER-KUBERNETES-STORAGE.md -->


---

<!-- SOURCE: 30-AI-INFRASTRUCTURE-ARCHITECTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** AI Infrastructure Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW-MEDIUM  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 30 AI INFRASTRUCTURE ARCHITECTURE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **AI Infrastructure Architecture**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Begin with workload type: training, fine-tuning, inference, RAG, data processing or mixed

Begin with workload type: training, fine-tuning, inference, RAG, data processing or mixed.

### 2. Model size, precision and runtime determine accelerator memory; context length, KV cache, batch/concurrency and tokens/sec materially affect inference sizing

Model size, precision and runtime determine accelerator memory; context length, KV cache, batch/concurrency and tokens/sec materially affect inference sizing.

### 3. Training requires attention to GPU topology, interconnect, collective communication, checkpointing, east-west bandwidth and storage throughput

Training requires attention to GPU topology, interconnect, collective communication, checkpointing, east-west bandwidth and storage throughput.

### 4. Storage design should distinguish datasets, checkpoints, model artifacts, vector/index data and general application data

Storage design should distinguish datasets, checkpoints, model artifacts, vector/index data and general application data.

### 5. Do not select GPU count from parameter count alone

Do not select GPU count from parameter count alone. Validate framework, model, precision, concurrency, latency/SLA and accelerator compatibility.

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


<!-- END SOURCE: 30-AI-INFRASTRUCTURE-ARCHITECTURE.md -->


---

<!-- SOURCE: 31-DATABASE-WORKLOAD-ARCHITECTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Database Workload Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW-MEDIUM  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 31 DATABASE WORKLOAD ARCHITECTURE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Database Workload Architecture**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Characterize transaction rate, read/write mix, block size, log behavior, working set, latency percentiles, growth and HA/DR requirements

Characterize transaction rate, read/write mix, block size, log behavior, working set, latency percentiles, growth and HA/DR requirements.

### 2. Data, logs, temp/work areas and backup may have different performance and recovery characteristics

Data, logs, temp/work areas and backup may have different performance and recovery characteristics.

### 3. Database licensing can dominate infrastructure economics; socket/core topology may materially affect TCO

Database licensing can dominate infrastructure economics; socket/core topology may materially affect TCO.

### 4. Application consistency, PITR, replication and backup mechanisms must align with database-native recovery semantics

Application consistency, PITR, replication and backup mechanisms must align with database-native recovery semantics.

### 5. Validate database version, OS/hypervisor, storage/network support, clustering and vendor certification

Validate database version, OS/hypervisor, storage/network support, clustering and vendor certification.

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


<!-- END SOURCE: 31-DATABASE-WORKLOAD-ARCHITECTURE.md -->

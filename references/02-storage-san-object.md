# 02 STORAGE SAN OBJECT

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 20-STORAGE-ARCHITECTURE-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Enterprise Storage Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for architecture principles; MEDIUM/HIGH for implementation limits and vendor-specific behavior  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Enterprise block/file/object storage architecture, discovery, sizing logic, resilience, performance, capacity, validation and design decision discipline

# 20 — STORAGE ARCHITECTURE KNOWLEDGE

## 1. Purpose

This pack gives ARCHON a vendor-neutral engineering foundation for enterprise storage architecture.

Its purpose is not to select a product by feature count. It is to translate business and workload requirements into measurable storage architecture drivers, identify missing evidence, expose contradictions, create defensible sizing targets, and determine when a design is mature enough to be mapped to a product family or physical configuration.

Core sequence:

**Business Requirement → Workload Characterization → Service Level → Capacity → Performance → Availability → Data Protection → Connectivity → Growth → Failure-State → Product Mapping → Physical Validation**

Product selection must not precede understanding of the workload unless the user explicitly requests a preliminary product hypothesis. Even then, the product remains a candidate until validated.

---

## 2. Core Architecture Principles

### 2.1 Architecture before product

Do not start with “Which array?” Start with:

- What data must be stored?
- How is it accessed?
- What performance is required?
- What happens when components or sites fail?
- How quickly must service/data recover?
- How much will the workload grow?
- What protection and cyber-recovery behavior is required?
- What infrastructure already exists?
- Which requirements are hard constraints versus preferences?

A named product supplied by the user is a candidate, not proof of suitability.

### 2.2 Capacity is not performance

A system capable of storing the required TB/PB may still fail latency, throughput, IOPS, rebuild, replication, recovery or failure-state requirements.

Likewise, a high-performance array may be unsuitable because of capacity economics, protocol support, recovery behavior, licensing, physical footprint or operational complexity.

Capacity sizing and performance sizing must be performed separately and reconciled later.

### 2.3 Normal-state success is insufficient

Enterprise designs must evaluate at minimum:

- normal operation,
- controller/node failure,
- media failure/rebuild,
- fabric/path failure,
- planned maintenance,
- replication degradation,
- site failure where applicable,
- backup/snapshot activity,
- recovery activity where material.

If an SLA applies during a failure state, the surviving architecture must satisfy that SLA.

### 2.4 Requirement, preference and assumption are different

Examples:

- “RPO=0” → requirement if formally confirmed.
- “We prefer IBM” → preference.
- “20% growth should be enough” → assumption unless supported by forecast.
- “3:1 reduction is normal” → assumption, not capacity evidence.
- “The current array is slow because disks are full” → hypothesis until telemetry supports it.

ARCHON must preserve these distinctions.

---

## 3. Storage Service Models

### 3.1 Block storage

Typical use cases:

- VMware/Hyper-V datastores
- databases
- transactional applications
- clustered filesystems
- enterprise applications

Common protocols:

- Fibre Channel
- iSCSI
- NVMe/FC
- NVMe/TCP

Primary concerns:

- latency
- multipathing
- queueing
- host/fabric design
- consistency
- volume/LUN design
- failure behavior

### 3.2 File storage

Typical use cases:

- user/shared data
- application shares
- engineering/media workloads
- home directories
- unstructured datasets

Common protocols:

- NFS
- SMB

Additional concerns:

- namespace
- metadata performance
- file count
- directory structure
- locking
- permissions/ACL
- small-file behavior
- scale-out namespace requirements

### 3.3 Object storage

Typical use cases:

- archive
- backup targets
- data lakes
- AI datasets
- application/object-native data
- large unstructured repositories

Typical access:

- S3-compatible APIs and vendor/object APIs

Additional concerns:

- object count
- object size distribution
- erasure coding
- consistency semantics
- namespace/bucket design
- API throughput
- immutability/object lock
- geographic durability

Do not assume block, file and object capacity/performance models are interchangeable.

---

## 4. Capacity Terminology

ARCHON must preserve these terms explicitly.

### Raw / Nominal Capacity

Aggregate manufacturer-rated media capacity before protection and system overhead.

Raw capacity is not usable capacity.

### Usable Capacity

Capacity available after applicable protection/system overhead as defined for the architecture.

Exact definitions can vary by vendor/tool. For procurement or compliance, use the vendor/configurator definition relevant to the offered configuration.

### Allocated / Provisioned Capacity

Logical capacity presented or assigned to hosts/applications.

Thin provisioning can make provisioned capacity exceed physically available usable capacity. This is not additional physical capacity.

### Consumed Capacity

Physical or logical capacity actually occupied, depending on the measurement context.

Always identify which definition the telemetry uses.

### Effective Capacity

A derived value based on assumed or observed data reduction such as compression/deduplication.

**Effective capacity must not be used to satisfy a hard usable-capacity requirement unless the requirement explicitly permits it.**

### Free-Space / Operational Headroom

Unallocated capacity intentionally retained for:

- growth,
- rebuild behavior,
- snapshots,
- performance stability,
- operational safety,
- migration,
- unexpected demand.

Headroom is not automatically a fixed percentage. It is a design parameter.

### TB versus TiB

Decimal and binary units differ materially at large scale.

If an RFP or sizing request is sensitive to capacity thresholds, clarify whether TB/TiB or PB/PiB is intended and use consistent units.

---

## 5. Capacity Sizing Logic

A generic capacity path:

**Current Required Usable → Growth → Protection/Operational Reserves → Snapshot/Clone Needs → Migration/Temporary Space → Final Required Usable**

Example growth model:

`Future Capacity = Current Capacity × (1 + CAGR)^Years`

If 500 TB current usable grows 12% annually for five years:

`500 × 1.12^5 ≈ 881 TB`

This is a growth projection, not a physical drive configuration.

If policy requires 20% free space after projected consumption:

`Required Usable = Projected Consumption / 0.80`

This is different from adding 20%:

`Projected × 1.20`

ARCHON should distinguish “20% extra capacity” from “maintain 20% free space.”

### Capacity items that may require separate reserves

- snapshots
- clones
- immutable recovery points
- replication journals/logs
- metadata
- migration coexistence
- rebuild/reserved areas
- system overhead
- test/dev copies
- backup staging

Do not assume snapshots consume zero capacity because they are pointer-based initially. Change rate and retention determine growth.

---

## 6. Data Reduction Discipline

Compression and deduplication can be valuable but are workload-dependent.

Factors affecting reducibility include:

- pre-compressed data
- encrypted data
- media files
- database compression
- backup data
- duplicate VM images
- zero patterns
- application-level compression/encryption
- dataset entropy

Rules:

1. Never silently apply a generic reduction ratio to hard usable requirements.
2. Separate guaranteed/program-backed reduction from marketing/example ratios.
3. Identify exclusions and workload eligibility.
4. If reduction is used for economics, show both physical usable and assumed effective capacity.
5. Treat observed reduction from a representative existing workload as stronger evidence than a generic ratio, while still accounting for future workload mix changes.

---

## 7. Workload Characterization

Storage performance cannot be represented by IOPS alone.

Minimum useful profile:

- IOPS: average, sustained peak, burst peak
- throughput: MB/s or GB/s
- block size / I/O size distribution
- read/write ratio
- sequential/random ratio
- latency requirement
- latency percentile: average, p95, p99, p99.9 where relevant
- queue depth / concurrency
- working-set size
- cache behavior
- dataset size
- peak duration
- time-of-day pattern
- growth
- failure-state SLA

### IOPS and throughput relationship

Approximation:

`Throughput ≈ IOPS × I/O Size`

Example:

`1,000,000 IOPS × 32 KiB ≈ 32.8 GB/s`

Therefore a high-IOPS requirement with large blocks may actually be a throughput requirement.

Conversely, a sequential 10 GB/s workload may require relatively few IOPS with large I/O sizes.

### Read/write mix matters

Writes can incur:

- protection overhead,
- cache destage,
- metadata work,
- replication traffic,
- snapshot/change tracking,
- parity calculations depending on implementation.

Do not compare two benchmark IOPS numbers unless workload conditions are sufficiently comparable.

---

## 8. Latency

Latency must specify:

- measurement point,
- read/write or combined,
- average versus percentile,
- normal versus failure state,
- replication state,
- workload conditions.

“Sub-millisecond latency” without workload context is not a complete requirement.

Averages can hide tail latency. For critical transactional workloads, p95/p99/p99.9 may be more meaningful.

Application latency is not identical to array latency. Host, hypervisor, HBA/NIC, SAN/network, queueing, filesystem, database and application behavior contribute to end-to-end latency.

---

## 9. Cache and Working Set

Cache can significantly influence benchmark results.

Questions:

- Is the benchmark read-hit dominated?
- Does the dataset fit in cache?
- Is the test steady-state?
- Are writes acknowledged from protected cache?
- What happens after cache saturation?
- What is the workload working-set size?
- Is compression/deduplication influencing cache efficiency?

Do not extrapolate a cache-friendly datasheet maximum directly to a production workload.

---

## 10. Front-End Connectivity

Architecture must consider both bandwidth and resilience.

Potential interfaces:

- FC
- NVMe/FC
- iSCSI
- NVMe/TCP
- NFS/SMB
- object/API connectivity

Questions:

- host count?
- ports per host?
- fabrics/networks?
- path count?
- HBA/NIC speed?
- switch capability?
- oversubscription?
- multipathing policy?
- controller port distribution?
- failure-state bandwidth?
- future host growth?

### Port-count anti-pattern

“More ports” does not automatically mean more performance.

A requirement such as 16×32G FC should be translated into:

- required aggregate throughput,
- redundancy,
- number of hosts,
- path/fabric architecture,
- surviving bandwidth after failures.

Physical port count can remain a procurement requirement, but it should have a technical rationale.

---

## 11. SAN and Multipathing

For enterprise block storage, eliminate avoidable single points of failure.

Typical design principles:

- independent fabrics where required,
- host paths distributed across fabrics/controllers,
- redundant HBAs/NICs,
- redundant switches,
- controlled zoning,
- supported multipathing software/policies,
- path-failure testing,
- documented maintenance behavior.

Do not treat “multiple paths visible” as proof of correct redundancy. Paths may share the same HBA, switch, controller, power domain or physical route.

Detailed FC/NVMe architecture belongs in `21-SAN-FC-NVME-KNOWLEDGE.md`.

---

## 12. Controller / Node Architecture

Terms such as dual-controller, active-active, active-passive and scale-out can have vendor-specific meanings.

Validate:

- ownership model,
- I/O forwarding behavior,
- cache protection,
- failover behavior,
- controller failure performance,
- non-disruptive maintenance behavior,
- maximum supported nodes/controllers,
- volume placement,
- path optimization,
- software/code upgrade behavior.

“Dual controller” alone does not prove zero interruption or full performance during failure.

---

## 13. Media Protection

Protection methods may include:

- mirroring
- RAID variants
- distributed RAID
- erasure coding
- vendor-specific schemes

Do not size protection using only textbook RAID formulas when vendor implementation uses distributed spare/rebuild areas, variable stripe geometry, metadata reservation or specific configuration rules.

Evaluate:

- usable efficiency
- tolerated failures
- correlated failure risk
- rebuild time
- rebuild performance impact
- media size
- failure domains
- minimum drive counts
- expansion behavior

Large-capacity media can increase rebuild exposure even when media reliability is high.

---

## 14. Rebuild and Degraded-State Design

A design that meets SLA only when healthy may not be enterprise-ready.

Ask:

- How long can rebuild take?
- What performance remains during rebuild?
- What happens if another media/controller/fabric component fails?
- Is rebuild traffic isolated or competing with host I/O?
- Is spare capacity distributed or dedicated?
- Does expansion change protection geometry?

Where important, acceptance testing should include degraded-state performance.

---

## 15. Snapshots and Clones

Snapshots are primarily storage-level recovery/operational mechanisms, not automatically backups.

Evaluate:

- crash consistency versus application consistency
- retention
- frequency
- change rate
- capacity impact
- immutable/protected status
- administrative separation
- replication
- restore granularity
- rollback semantics
- ransomware threat model

### Snapshot ≠ Backup

A snapshot on the same failure/security domain may be lost or compromised with the primary system.

Backup architecture should consider independent failure and security domains.

---

## 16. Replication

Replication design requires more than “sync or async.”

Discovery:

- RPO
- RTO
- dataset scope
- write rate/change rate
- RTT
- available bandwidth
- dedicated/shared link
- consistency requirements
- failover mode
- failback
- witness/quorum
- split-brain protection
- planned migration behavior
- test-DR requirements
- cyber-recovery workflow

### Synchronous replication

Typically couples remote acknowledgement to write completion semantics. Distance/RTT therefore matters.

Do not promise “zero latency impact” as a generic property.

### Asynchronous replication

Can tolerate longer RTT but introduces non-zero data-loss exposure according to implementation/policy.

Do not infer RPO solely from the word “asynchronous.”

Detailed treatment belongs in `26-REPLICATION-DR-ARCHITECTURE.md`.

---

## 17. Availability and Disaster Recovery

Availability is not one number.

Separate:

- component availability
- array availability
- application availability
- data availability
- site availability
- recoverability

A dual-controller array does not solve site disaster.

Two arrays do not automatically create application HA.

Replication does not automatically provide automatic failover.

RPO does not define RTO.

Storage HA must be aligned with compute, network, DNS/load-balancing, application and operational recovery.

---

## 18. Cyber Resilience

Cyber resilience should be decomposed into:

**Prevent → Detect → Protect → Respond → Recover → Validate**

Storage-relevant capabilities may include:

- immutable/protected recovery points
- anomaly detection
- administrative separation
- MFA/integration controls
- audit logging
- secure replication
- isolated copies
- recovery-point analysis
- clean-room workflows
- recovery validation

“Immutable snapshot” alone is not a complete ransomware strategy.

Detailed treatment belongs in `25-CYBER-RESILIENCE-ARCHITECTURE.md`.

---

## 19. Backup Interaction

Production storage and backup storage solve different problems.

Do not derive backup repository capacity directly from production TB without at least:

- protected dataset
- daily change rate
- retention
- backup method
- full/incremental policy
- GFS
- compression/dedupe
- immutability
- copy/offsite policy
- backup window
- ingest rate
- restore SLA
- Instant Recovery requirements

Production snapshots can complement backup but do not automatically replace it.

---

## 20. VMware / Virtualization Considerations

For virtualized workloads consider:

- datastore count and size
- VM density
- failure blast radius
- queueing
- multipathing
- storage policy
- replication grouping
- consistency grouping
- snapshot/backup integration
- vVol/VMFS/NFS architecture where applicable
- operational limits
- recovery orchestration

Avoid both extremes:

- one giant datastore without operational rationale
- excessive fragmentation into many tiny datastores without need

Datastore design should align with failure, recovery, performance and operational boundaries.

---

## 21. Database Workloads

Database requirements can be sensitive to:

- write latency
- log latency
- I/O consistency
- burst behavior
- block size
- sequential scans
- checkpoint behavior
- application consistency
- multipathing
- certification/support
- replication semantics

Do not assume database vendor support merely because the array presents standard block storage.

Certification/support requirements may be version-specific.

---

## 22. Scale-Up versus Scale-Out

### Scale-Up
Adds capacity/performance within an existing controller/system boundary.

Advantages may include simplicity and fewer management objects.

Risks may include controller ceiling and larger failure concentration.

### Scale-Out
Adds nodes/controllers/systems to expand resources.

Advantages may include broader scaling and distribution.

Risks may include balancing, licensing, network complexity and workload placement constraints.

Do not assume “scale-out” means linear scaling.

Validate scaling behavior under the relevant workload.

---

## 23. Consolidation versus Isolation

Consolidation can improve utilization and operations but increases shared blast radius.

Consider isolation when required by:

- security
- regulatory boundaries
- performance predictability
- licensing
- administrative separation
- lifecycle differences
- recovery requirements
- workload incompatibility

Avoid creating separate arrays solely because applications have different names. Conversely, avoid consolidation solely for utilization efficiency when failure/security requirements conflict.

---

## 24. Five-Year Design

A five-year requirement must define growth.

At minimum consider:

- capacity CAGR
- performance CAGR
- host/VM growth
- workload mix changes
- snapshot/retention growth
- replication growth
- software/platform evolution
- support lifecycle
- interface evolution
- expansion limits

“Must support all growth for five years” is unmeasurable without a forecast or bounded scenario.

---

## 25. Physical and Operational Constraints

Discovery may include:

- rack units
- rack depth
- power
- cooling
- weight
- datacenter standards
- port availability
- cable/transceiver standards
- power feeds
- management network
- encryption/key management
- monitoring
- remote support policy
- air-gap/internet restrictions

Physical feasibility must be validated before an orderable BOM.

---

## 26. Architecture Decision Drivers

Rank drivers where possible:

1. hard compliance requirements
2. availability/RPO/RTO
3. performance/latency
4. capacity/growth
5. security/cyber recovery
6. interoperability
7. operational simplicity
8. lifecycle/support
9. commercial/licensing
10. preference/standardization

The ordering may change by customer. ARCHON should not invent priority if the customer has not supplied it.

---

## 27. Discovery Minimums

Before moving from conceptual to budgetary design, seek enough evidence on:

### Capacity
- current used/usable
- required net usable
- growth
- snapshots/clones
- data reduction treatment

### Performance
- IOPS
- throughput
- block size
- R/W
- latency
- peak duration
- percentiles where available

### Availability
- RPO
- RTO
- failure-state SLA
- site model

### Connectivity
- hosts
- protocol
- HBA/NIC
- fabrics/switches
- port speeds

### Protection
- backup
- snapshots
- immutability
- replication

### Operations
- maintenance windows
- monitoring
- migration
- support expectations

Not every project requires every field before any useful work can begin. Missing material inputs must be visible and reflected in confidence.

---

## 28. Sizing Confidence Levels

### LOW
Major workload/capacity/availability inputs missing.

Allowed:
- conceptual architecture
- discovery questions
- scenarios

Not allowed:
- authoritative physical configuration

### MEDIUM
Enough inputs for budgetary architecture but some material vendor/configuration details remain.

Allowed:
- capacity target
- architecture topology
- approximate resource class
- budgetary scenarios

Not allowed:
- unsupported orderable BOM

### HIGH
Requirements, workload evidence, vendor rules, interoperability and physical configuration have been validated sufficiently for the stated decision.

High confidence is decision-specific. A design can have high confidence in architecture and lower confidence in exact BOM.

---

## 29. Configuration Maturity

### Level 0 — Discovery
Problem definition and missing inputs.

### Level 1 — Conceptual Architecture
Technology topology and design principles.

### Level 2 — Budgetary / Not Orderable
Sizing targets and approximate product/resource class.

### Level 3 — Validated Physical Configuration
Vendor configuration rules, capacities, connectivity and support validated.

### Level 4 — Orderable BOM
Exact part numbers, quantities, licenses, support and required accessories validated.

Never label a Level 2 design as Level 4 because arithmetic looks plausible.

---

## 30. Product Mapping

Once architecture drivers are sufficiently understood, map products based on:

- usable capacity envelope
- performance envelope
- latency
- connectivity
- availability
- replication
- cyber resilience
- interoperability
- expansion
- lifecycle
- support
- commercial model

A product family can be a valid candidate before exact configuration is known.

### Capability versus Offered Configuration

Always distinguish:

**Product Capability:** Can the product family/architecture support the requirement?

**Offered Configuration:** Does the exact proposed configuration actually include and satisfy it?

A product may be capable of 16 FC ports while the offered BOM contains only 8. Capability PASS is not offered-configuration PASS.

---

## 31. Evidence Hierarchy

For exact product/compliance claims prefer:

1. current vendor configuration/ordering tool
2. current official product documentation / support matrix
3. current official interoperability/HCL
4. official technical papers / validated designs
5. reputable independent testing where relevant
6. distributor/partner material
7. secondary summaries

Marketing claims should be identified as such when test conditions are material.

Exact limits, SKU availability, licensing and support matrices are volatile and should be revalidated.

---

## 32. Common Anti-Patterns

ARCHON should challenge:

- sizing only from total TB
- sizing only from average IOPS
- treating average CPU/storage load as peak requirement
- using effective capacity as usable
- assuming a generic reduction ratio
- “snapshot = backup”
- “replication = backup”
- “dual controller = DR”
- “more ports = more performance”
- “all-flash = latency requirement automatically met”
- comparing datasheet maximum IOPS across different workloads
- ignoring controller/fabric/site failure states
- ignoring rebuild behavior
- using current workload without growth
- assuming all workloads grow at the same rate
- exact SKU selection before telemetry
- treating customer preference as technical evidence
- treating vendor capability as offered BOM compliance

---

## 33. Architecture Challenge Questions

When reviewing a proposed storage design, ask:

1. What requirement would cause this design to fail?
2. What happens during controller failure?
3. What happens during fabric/path failure?
4. What happens during media rebuild?
5. What happens when replication is active?
6. What happens during backup/snapshot activity?
7. What happens at year 5?
8. Which assumption has the greatest sizing impact?
9. Which claim depends on data reduction?
10. Which capability is only datasheet-level and not yet validated?
11. What is the recovery behavior after ransomware?
12. Can the proposed network carry the replicated write workload?
13. What is the largest operational blast radius?
14. What must be tested in a POC rather than assumed?

---

## 34. POC / Validation Principles

A meaningful storage POC should reproduce relevant conditions rather than chase a headline benchmark.

Potential dimensions:

- representative dataset
- representative block sizes
- R/W mix
- concurrency
- steady-state duration
- p95/p99 latency
- cache-warm and cache-stressed conditions
- snapshots active
- replication active
- backup interaction
- controller failure
- path/fabric failure
- rebuild/degraded state
- recovery operation

Acceptance criteria must be measurable before the test begins.

---

## 35. RFP Guidance

Prefer outcome-based requirements.

Better:
“System shall sustain X GB/s and Y IOPS at specified workload and p99 latency under defined failure state.”

Weaker:
“System shall contain at least 24 SSDs.”

Better:
“System shall provide ≥500 TB net production usable after protection, excluding snapshot reserve.”

Weaker:
“System shall provide 1.5 PB effective.”

Physical requirements are valid when driven by genuine rack, power, standardization or procurement constraints, but their rationale should be understood.

---

## 36. Escalation / TBD Conditions

ARCHON should explicitly keep a point TBD when:

- capacity terminology is ambiguous
- workload telemetry is missing
- failure-state SLA is unknown
- replication RTT/bandwidth is unknown
- product feature is version-dependent
- exact drive/adapter population is not validated
- HCL/interoperability is unknown
- licensing/commercial model changes the architecture
- customer requirements contradict each other
- acceptance criteria cannot be measured

TBD is preferable to false precision.

---

## 37. Final Decision Discipline

A strong enterprise storage answer should make visible:

**Known Facts**  
What is supported by customer data or authoritative evidence.

**Assumptions**  
What is temporarily assumed for scenario analysis.

**Calculations**  
Transparent arithmetic with units.

**Architecture Decisions**  
Choices supported by requirements.

**Open Questions**  
Missing information that can materially change the design.

**Validation Items**  
What must be checked against current vendor/HCL/configurator evidence.

**Confidence**  
How mature the current conclusion is.

The objective is not to produce the most detailed configuration fastest.

The objective is to produce the **most defensible architecture at the current evidence level**, then increase configuration specificity as evidence improves.


<!-- END SOURCE: 20-STORAGE-ARCHITECTURE-KNOWLEDGE.md -->


---

<!-- SOURCE: 21-SAN-FC-NVME-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** SAN, Fibre Channel & NVMe Fabrics  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for fabric principles; MEDIUM/HIGH for vendor-specific limits, optics, firmware, HCL and feature support  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** FC SAN, zoning, fabrics, ISLs, multipathing, HBA/storage ports, bandwidth, congestion, queueing, NVMe/FC, NVMe/TCP, failure-state design and validation

# 21 — SAN / FC / NVMe KNOWLEDGE

## 1. Purpose

This pack gives ARCHON a vendor-neutral framework for designing and reviewing enterprise storage connectivity.

Core principle:

**A SAN is not a collection of ports. It is an end-to-end I/O path with bandwidth, latency, queueing, failure domains and interoperability constraints.**

Reasoning path:

**Host Workload → Host Adapter → Fabric/Network → Storage Port → Controller → Backend → Failure State → Multipathing → Validation**

---

## 2. Protocol Families

### Fibre Channel
Purpose-built storage fabric commonly used for enterprise block storage.

Typical generations include 16G, 32G and 64G FC, subject to platform support.

### NVMe over Fibre Channel
Transports NVMe commands over FC fabrics.

Potential benefits include reduced protocol overhead and improved parallelism/queue behavior.

NVMe/FC requires end-to-end support from host OS/hypervisor, HBA, switch/fabric and storage.

### iSCSI
SCSI block protocol over IP/Ethernet.

### NVMe/TCP
NVMe over standard TCP/IP Ethernet networks.

Protocol choice should follow requirements and existing infrastructure, not fashion.

---

## 3. End-to-End Compatibility

A link is supported only when the complete stack is supported.

Validate:

- server
- operating system/hypervisor
- HBA/NIC
- driver
- firmware
- transceiver/optic
- switch
- switch firmware
- storage adapter
- storage software/code
- multipathing
- protocol mode

A storage port supporting 64G FC does not make a 32G host path operate at 64G.

Exact interoperability is version-sensitive and should be checked against current HCL/support matrices.

---

## 4. Speed Negotiation

FC links generally negotiate to a mutually supported speed subject to hardware and topology constraints.

Example:

A storage-side port capable of 32G/64G may operate at 32G when connected through a supported 32G path.

Do not assume cross-generation interoperability without validating:

- supported speeds
- optics
- switch port capability
- HBA
- vendor support matrix

Technical link-up and vendor-supported configuration are different questions.

---

## 5. Dual-Fabric Principle

A common enterprise pattern uses two independent fabrics:

**Fabric A**  
**Fabric B**

Each host and storage system has paths through both.

Independence should include, where required:

- separate switches
- separate HBAs/ports
- separate storage ports
- separate power
- separate ISLs
- separate physical routes

Dual fabric is valuable only if one fabric can fail without violating the required service level.

---

## 6. Path Diversity

Multiple paths are not necessarily diverse.

Four paths can still share:

- one HBA
- one switch
- one ISL
- one storage adapter
- one controller
- one cable route
- one power domain

ARCHON should map paths to actual failure domains.

---

## 7. Multipathing

Multipathing provides path redundancy and may provide load distribution.

Validate:

- supported MPIO/native multipathing
- path selection policy
- ALUA/ANA behavior where relevant
- optimized/non-optimized paths
- failover timeout
- queue behavior
- path recovery
- vendor best practices

“Four visible paths” is not proof of correct multipathing.

---

## 8. Path Count

Too few paths can reduce resilience.

Too many paths can increase:

- operational complexity
- login/session count
- queue interactions
- zoning complexity
- troubleshooting difficulty

Path count should be sufficient for redundancy and bandwidth, not maximized without reason.

---

## 9. Zoning

Zoning controls initiator/target visibility within FC fabrics.

Common design goals:

- fault isolation
- predictable discovery
- security boundary improvement
- simpler troubleshooting
- controlled change

A commonly preferred pattern is small, explicit zones such as single-initiator zoning, subject to vendor guidance.

Exact zoning requirements should follow current platform/vendor best practices.

---

## 10. Zoning Is Not LUN Masking

Zoning controls fabric visibility.

LUN masking/mapping controls which storage resources are presented to hosts.

Both may be required.

Do not treat one as a substitute for the other.

---

## 11. Fabric Login and Scale

Large fabrics must consider:

- initiator count
- target count
- login/session limits
- zoning scale
- name server behavior
- switch/domain limits
- NPIV
- virtualization density

Exact limits are vendor-specific and volatile.

---

## 12. HBA Design

For each host evaluate:

- number of HBAs
- ports per HBA
- PCIe placement
- NUMA locality where material
- supported speed
- firmware/driver
- queue depth
- boot-from-SAN requirements
- fabric distribution

Two dual-port HBAs do not automatically mean all four ports should be used for storage.

Design based on bandwidth, resilience and supportability.

---

## 13. PCIe Considerations

Adapter performance can be constrained by:

- PCIe generation
- lane width
- slot wiring
- shared risers
- CPU/socket affinity
- other high-bandwidth adapters

A nominal 64G FC or high-speed NIC does not guarantee equivalent host throughput if the PCIe path is constrained.

Physical server configuration must be validated.

---

## 14. Storage Front-End Ports

Evaluate:

- port speed
- protocol
- controller ownership
- adapter/card placement
- fabric distribution
- host count
- expected throughput
- failure-state bandwidth
- supported port combinations

Do not size storage ports solely from the number of host HBA ports.

---

## 15. Aggregate Bandwidth

A simple theoretical calculation:

`Aggregate Nominal Bandwidth = Port Count × Port Speed`

This is only an upper-bound transport figure.

Real throughput is affected by:

- encoding/protocol overhead
- workload
- controller limits
- PCIe
- fabric
- backend
- queueing
- path policy
- failure state

Do not convert nominal line rate directly into guaranteed application throughput.

---

## 16. GB/s versus Gbps

Preserve units.

Approximation:

`1 GB/s ≈ 8 Gbps` before protocol overhead.

Example:

8 GB/s application throughput ≈ 64 Gbps payload rate.

This immediately shows why a single 32G link cannot carry an 8 GB/s workload.

But real design must include overhead and redundancy.

---

## 17. Port Utilization

Port utilization should be evaluated over time.

Look for:

- sustained utilization
- bursts
- imbalance
- congestion
- errors
- credit starvation
- queue buildup
- failure-state utilization

Low average utilization does not rule out short saturation events.

---

## 18. Oversubscription

Oversubscription is acceptable when intentional and supported by workload concurrency.

Example areas:

- host ports to switch uplinks
- edge to core
- ISLs
- storage front-end ports

Ask:

- What percentage of hosts can peak simultaneously?
- What is the failure-state ratio?
- What is the workload SLA?
- What happens during backup/migration?

Avoid both extremes: accidental severe oversubscription and unnecessary 1:1 provisioning everywhere.

---

## 19. ISL Design

Inter-Switch Links can become hidden bottlenecks.

Evaluate:

- number/speed
- trunking
- oversubscription
- traffic locality
- fabric topology
- failure state
- distance
- latency
- buffer/credit behavior

Where possible, keep host-to-storage traffic local within the intended fabric topology rather than forcing unnecessary ISL traversal.

---

## 20. Long-Distance FC

Distance can affect:

- latency
- buffer credits
- throughput
- optics
- extension technology
- error behavior

Do not assume metro/DCI FC behaves like local SAN.

For replication, IP-based transport may be architecturally different from host FC connectivity.

---

## 21. Buffer-to-Buffer Credits

FC uses credit-based flow control.

Long distance or congestion can require attention to buffer credits.

Insufficient credits can reduce throughput even when nominal link bandwidth appears sufficient.

Exact configuration is switch/vendor-specific.

---

## 22. Congestion

Potential sources:

- slow-drain devices
- overloaded ISLs
- misbehaving hosts
- credit starvation
- queue saturation
- speed mismatch
- oversubscription

Symptoms can include increased latency, frame delay and performance instability.

SAN troubleshooting should use fabric telemetry, not only storage-array metrics.

---

## 23. Slow Drain

A slow-drain device consumes frames more slowly than expected and can propagate congestion.

Investigate:

- host behavior
- HBA/driver/firmware
- queueing
- application pauses
- fabric counters
- credit behavior

Do not immediately blame the storage array for end-to-end latency.

---

## 24. Queue Depth

Queue depth affects concurrency and can influence performance and latency.

Too low:
- underutilization

Too high:
- excessive queueing
- tail latency
- unfairness

Queue depth exists at multiple layers:

- application
- OS
- hypervisor
- HBA
- switch
- storage

Do not tune blindly. Follow validated vendor/application guidance.

---

## 25. FC Error Analysis

Relevant indicators may include:

- CRC errors
- link resets
- loss of sync
- encoding errors
- credit issues
- link flaps

Errors can originate from:

- optics
- cable
- patch panel
- dirty connectors
- HBA
- switch port
- storage port

Physical-layer health matters.

---

## 26. Optics and Cabling

Validate:

- SR/LR optics
- wavelength
- fiber type
- connector
- distance
- patch panels
- vendor support
- cleanliness

A logically correct SAN can still fail due to unsupported or poor physical media.

---

## 27. NVMe/FC

NVMe/FC uses FC transport while replacing SCSI command semantics with NVMe.

Potential design benefits:

- efficient queues
- parallelism
- lower protocol overhead

Requirements:

- supported HBA
- supported switch/fabric
- supported storage
- OS/hypervisor support
- multipathing/ANA support
- correct drivers/firmware

Do not claim NVMe/FC automatically produces a specific latency improvement. Benefit is workload/platform dependent.

---

## 28. NVMe/TCP

NVMe/TCP runs over standard IP/Ethernet.

Evaluate:

- NIC speed
- CPU overhead
- network design
- latency
- loss/congestion
- multipathing
- VLAN/routing
- MTU
- QoS
- switch buffers
- DCB requirements if any for the specific design

Do not import RoCE assumptions into NVMe/TCP.

---

## 29. NVMe/RoCE Distinction

NVMe over RoCE and NVMe/TCP are different transports.

RoCE typically requires more deliberate lossless/congestion-control design.

NVMe/TCP uses TCP and does not require a lossless Ethernet fabric in the same manner.

Do not use “NVMe over Ethernet” as if all transports have identical requirements.

---

## 30. FC versus NVMe/FC Migration

Existing FC infrastructure may sometimes support transition to NVMe/FC, but validate the complete stack.

Questions:

- Are HBAs NVMe/FC capable?
- Switch firmware/features?
- Storage ports?
- Hypervisor/OS?
- coexistence with FCP?
- operational tooling?
- HCL?

A future migration path can be a design advantage, but should not be promised without evidence.

---

## 31. FC versus Ethernet Decision

Consider:

- installed skills/infrastructure
- workload latency
- protocol requirements
- operational model
- convergence strategy
- cost
- scalability
- security
- support ecosystem

There is no universal winner.

A customer with mature dual-fabric FC should not be moved to Ethernet merely because a newer protocol exists.

---

## 32. VMware Considerations

Validate:

- ESXi version/build
- HBA/NIC
- driver/firmware
- storage code
- multipathing policy
- SATP/PSP where applicable
- NVMe support
- datastore protocol
- queue behavior
- HCL

“VMware supported” is too broad. Exact stack support matters.

---

## 33. Boot from SAN

Boot-from-SAN can simplify some operational models but introduces dependencies.

Validate:

- HBA support
- zoning
- LUN masking
- path behavior
- boot order
- recovery procedures
- cluster operations

It should be an intentional architecture decision.

---

## 34. Failure-State Bandwidth

If a dual-fabric design normally carries 50% of traffic on each fabric, losing one may force 100% through the survivor.

Therefore:

**Normal utilization must not be the only bandwidth check.**

Evaluate:

`Surviving Fabric Capacity >= Required Failure-State Traffic`

under the defined SLA.

---

## 35. Controller + Fabric Combined Failure

Where business criticality warrants, evaluate combined scenarios.

Example:

- one storage controller unavailable for maintenance
- then one SAN fabric fails

This can expose hidden path concentration.

Do not assume every multi-failure scenario must be designed out; document residual risk and business requirement.

---

## 36. Port Count RFPs

Challenge arbitrary requirements such as:

“Minimum 16×32G FC ports.”

Translate into outcomes:

- host count
- path redundancy
- aggregate throughput
- failure-state throughput
- growth
- fabric design

A physical port-count requirement may still be valid, but should not replace performance architecture.

---

## 37. Fabric Topology

Possible architectures include:

- single-switch fabric
- dual independent fabrics
- core-edge
- director-based
- stretched/extended fabrics

Choose based on:

- scale
- availability
- port density
- distance
- operational model
- growth
- failure domains

Do not prescribe topology from port count alone.

---

## 38. SAN Security

Consider:

- zoning
- LUN masking
- management-plane security
- role-based access
- fabric segmentation
- secure management protocols
- logging
- firmware governance

SAN isolation is not a replacement for host/application security.

---

## 39. Monitoring

Useful telemetry includes:

- port throughput
- utilization
- errors
- congestion
- credits
- latency where available
- link resets
- path state
- ISL utilization
- queue metrics

Correlate host, fabric and storage telemetry to locate bottlenecks.

---

## 40. Troubleshooting Sequence

A practical sequence:

1. Confirm scope and time.
2. Check application/host latency.
3. Check multipath/path state.
4. Check HBA/NIC errors/queues.
5. Check fabric port/ISL health.
6. Check storage front-end ports.
7. Check controller/backend.
8. Correlate with workload events.
9. Compare normal vs affected period.

Avoid starting with a component blame assumption.

---

## 41. Discovery Inputs

Before SAN design seek:

- host count
- OS/hypervisor
- HBA/NIC inventory
- port speeds
- switch models
- firmware
- fabric topology
- existing zoning
- storage target ports
- workload throughput/IOPS
- failure-state SLA
- growth
- distance
- replication transport
- maintenance constraints

---

## 42. Validation Gates

### Conceptual
Protocol and fabric architecture selected.

### Budgetary
Approximate port counts/speeds and topology identified.

### Validated Physical
Exact HBA, switch, optic, cable, storage adapter, firmware/driver and HCL validated.

### Orderable
All part numbers, quantities, licenses, optics/cables and support dependencies confirmed.

Do not turn a conceptual “8 FC ports should be enough” into an orderable SAN BOM.

---

## 43. Evidence Requirements

Highly volatile items require current authoritative evidence:

- supported HBA
- supported driver/firmware
- switch interoperability
- optic/transceiver support
- maximum ports/logins
- NVMe/FC support
- speed compatibility
- HCL
- exact storage adapter population

Prefer current vendor HCL/configuration tools and official documentation.

---

## 44. Anti-Patterns

Challenge:

- more paths = always better
- more ports = always faster
- dual fabric = automatically independent
- 64G storage port makes 32G host run at 64G
- nominal bandwidth = application throughput
- low average port utilization = no congestion
- zoning = LUN masking
- NVMe/FC = guaranteed lower latency
- NVMe/TCP = RoCE
- link up = supported configuration
- no array alert = SAN healthy
- FC issue = storage issue
- normal-state bandwidth = failure-state bandwidth
- two PSUs/switches/paths = independent failure domains without verification

---

## 45. Architecture Challenge Questions

1. If Fabric A disappears, can Fabric B carry the required workload?
2. Are paths truly independent?
3. Which traffic crosses ISLs?
4. Where is oversubscription?
5. What is the narrowest link?
6. Are host and storage speeds actually interoperable?
7. Is the exact driver/firmware/HBA/storage stack supported?
8. Could a slow-drain device affect other workloads?
9. Are queue depths evidence-based?
10. What happens during controller + fabric degradation?
11. Is NVMe migration end-to-end supported?
12. Are optics/cabling/distance validated?
13. Does the design still work at year 5?
14. Which claims require current HCL evidence?

---

## 46. Final Principle

ARCHON should not design SAN by counting ports.

It should design an **end-to-end resilient I/O path**.

The engineering sequence is:

**Characterize Workload → Select Protocol → Map Paths → Separate Failure Domains → Calculate Bandwidth → Check Oversubscription → Validate Multipathing → Validate Interoperability → Test Failure State → Build Physical BOM**

Only after the complete path is validated should SAN connectivity be considered orderable.


<!-- END SOURCE: 21-SAN-FC-NVME-KNOWLEDGE.md -->


---

<!-- SOURCE: 45-OBJECT-STORAGE-KNOWLEDGE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Object Storage Knowledge  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW-MEDIUM  
**EVIDENCE CLASS:** ARCHON engineering / product knowledge layer  
**SCOPE:** Architecture-first decision support; exact current product, licensing, interoperability and ordering claims require authoritative validation where applicable.

# 45 OBJECT STORAGE KNOWLEDGE

## 1. Purpose

This pack extends ARCHON with structured knowledge for **Object Storage Knowledge**. It complements the Core Runtime and existing discovery, sizing, evidence, assumption, RFP, BOM and anti-hallucination frameworks.

ARCHON must preserve the maturity sequence:

**Discovery → Conceptual Architecture → Budgetary / Not Orderable → Validated Physical Configuration → Orderable BOM / Operational Readiness**

## 2. Core Knowledge

### 1. Size object storage using usable capacity, erasure coding/protection, object count, object size distribution, API throughput, metadata behavior and growth

Size object storage using usable capacity, erasure coding/protection, object count, object size distribution, API throughput, metadata behavior and growth.

### 2. Evaluate S3 compatibility at API/feature level, not from the label alone

Evaluate S3 compatibility at API/feature level, not from the label alone.

### 3. Consider immutability/object lock, versioning, lifecycle, replication, consistency, namespace, multi-tenancy and failure domains

Consider immutability/object lock, versioning, lifecycle, replication, consistency, namespace, multi-tenancy and failure domains.

### 4. Small-object workloads can behave very differently from large sequential objects

Small-object workloads can behave very differently from large sequential objects.

### 5. Validate exact platform limits, EC schemes, node minimums, expansion rules and software compatibility

Validate exact platform limits, EC schemes, node minimums, expansion rules and software compatibility.

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


<!-- END SOURCE: 45-OBJECT-STORAGE-KNOWLEDGE.md -->

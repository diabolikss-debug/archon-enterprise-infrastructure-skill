# 06 CAPACITY PERFORMANCE HA

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 32-CAPACITY-PERFORMANCE-ENGINEERING.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Capacity & Performance Engineering  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for engineering methods; MEDIUM/HIGH for vendor benchmark limits  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Storage/compute/infrastructure capacity forecasting, workload normalization, IOPS-throughput-latency relationships, headroom, failure-state sizing, scenario analysis, telemetry quality and validation

# 32 — CAPACITY & PERFORMANCE ENGINEERING

## 1. Purpose

This pack defines how ARCHON converts incomplete infrastructure measurements into defensible capacity and performance requirements without creating false precision.

Core principle:

**Measurement → Normalize → Forecast → Add service-level constraints → Model failure state → Add operational headroom → Validate against product/configuration evidence**

A mathematical result is not automatically a validated configuration.

---

## 2. Evidence Quality

### High-quality evidence
- 30–90 day time-series telemetry
- sustained peak / p95 / p99 measurements
- workload-specific counters
- observed growth history
- application SLA
- current topology and failure-state data
- vendor-validated configuration/performance evidence

### Medium-quality evidence
- recent peak measurements
- representative monitoring samples
- customer capacity forecast
- known workload ratios

### Low-quality evidence
- averages without time distribution
- VM count alone
- total TB alone
- “usually low”
- generic data-reduction ratios
- marketing maximums
- undocumented growth estimates

Low-quality inputs may support scenarios, not high-confidence sizing.

---

## 3. Average, Peak and Percentiles

Average utilization is useful for trend context but weak for sizing burst-sensitive infrastructure.

Prefer, when available:

- sustained peak
- p95
- p99
- peak duration
- frequency of peaks
- seasonality
- maintenance/batch windows

A 35% CPU average can coexist with 90% business-hour peaks.

A 100K average IOPS workload can have 300K sustained peaks.

Do not size a critical system solely from averages.

---

## 4. Growth Mathematics

Generic compound growth:

`Future = Current × (1 + CAGR)^Years`

Examples:

- 500 TB at 12% for 5 years → ~881 TB
- 100 units at 20% for 5 years → ~249 units

Growth assumptions should be applied only to the metric they describe.

If the customer says “20% annual growth,” clarify whether this means:

- capacity,
- VM count,
- users,
- transactions,
- CPU demand,
- memory,
- IOPS,
- all workloads.

Do not automatically apply one CAGR to every resource.

---

## 5. Headroom

Headroom protects against uncertainty, bursts, maintenance and failure-state conditions.

Two different statements:

**Add 20% capacity:**  
`Required = Demand × 1.20`

**Keep 20% free after demand:**  
`Required = Demand / 0.80`

These are not equivalent.

Headroom should have a reason. Avoid a universal percentage.

Potential reasons:

- growth uncertainty
- workload burst
- rebuild
- controller/node loss
- snapshot growth
- migration
- temporary coexistence
- operational policy

---

## 6. Failure-State Sizing

Normal-state capacity/performance is not sufficient when the SLA must survive failures.

For an N-node homogeneous cluster with one-node failure:

`Surviving Nodes = N - 1`

A simple capacity check:

`Required Per Surviving Node = Failure-State Demand / (N - 1)`

But performance may not scale linearly with node count.

Validate:

- controller/node ownership
- NUMA
- cache
- network
- fabric
- storage backend
- licensing
- application placement

N+1 is a topology statement, not proof of SLA compliance.

---

## 7. CPU Sizing Discipline

Do not treat cores from different CPU generations/models as equal performance units.

A rough current demand proxy may be:

`Current Consumed Core-Equivalent ≈ Installed Cores × Relevant Utilization`

This is only a proxy tied to the current CPU generation.

Future demand proxy:

`Future Old-Core-Equivalent = Current Proxy × Growth Factor`

To map to a new CPU, use:

- representative benchmark
- workload-specific benchmark
- SPEC results where appropriate
- application vendor guidance
- measured POC
- credible per-core performance normalization

Do not silently assume “one new core = one old core.”

### CPU metrics to seek

- p95/p99 CPU utilization
- CPU ready/co-stop for virtualization
- clock/frequency behavior
- socket/core topology
- NUMA
- largest VM
- vCPU distribution
- oversubscription
- application licensing constraints

---

## 8. Memory Sizing

Memory often becomes the limiting resource when server count is reduced.

Current active memory proxy:

`Installed Memory × Relevant Utilization`

Future memory:

`Current Active Memory × Growth Factor`

Then evaluate:

- hypervisor/system overhead
- N+1
- reserved memory
- large-memory VM placement
- NUMA boundaries
- DIMM population
- future expansion
- memory licensing where relevant

Do not infer future RAM demand from CPU growth unless explicitly justified.

---

## 9. VM Count

VM count is an inventory metric, not a compute requirement.

650 VMs may be lighter than 100 database VMs.

Use VM count for:

- operational scale
- management
- backup job design
- migration planning
- datastore/failure-domain planning

Use telemetry for compute sizing.

---

## 10. Storage Performance Triangle

Always relate:

**IOPS ↔ I/O Size ↔ Throughput**

Approximation:

`Throughput = IOPS × I/O Size`

Useful conversions:

- 1 KiB = 1024 bytes
- 1 MiB = 1024 KiB
- 1 GiB = 1024 MiB
- 1 byte = 8 bits

Example:

`1,000,000 × 32 KiB ≈ 32.8 GB/s`

A requirement that appears to be “IOPS” may actually be constrained by bandwidth.

---

## 11. Read/Write Mix

Example:

8 GB/s total workload at 70% read / 30% write:

- Read ≈ 5.6 GB/s
- Write ≈ 2.4 GB/s

A rough uncompressed logical write bandwidth proxy:

`2.4 GB/s × 8 ≈ 19.2 Gbps`

This is not automatically replication WAN bandwidth.

Replication behavior may include:

- compression
- coalescing
- metadata
- protocol overhead
- changed-block behavior
- write amplification
- journal/log behavior

Use this calculation as an architecture check, not a final WAN sizing formula.

---

## 12. Latency Engineering

Latency should identify:

- host/application or array measurement point
- read/write
- average/p95/p99
- workload level
- normal/failure state
- replication state

As utilization approaches a resource ceiling, queueing can cause nonlinear latency growth.

Therefore “system can deliver X maximum IOPS” does not imply acceptable latency at X.

Performance target should ideally be written:

**IOPS + throughput + workload profile + latency percentile + test duration + failure state**

---

## 13. Queueing and Saturation

Watch for:

- increasing queue depth
- rising latency while IOPS plateaus
- host queue saturation
- HBA/NIC saturation
- fabric congestion
- controller saturation
- backend/media saturation

A system may appear below a marketing IOPS maximum while another component is already the bottleneck.

---

## 14. Bandwidth Engineering

Check every relevant path:

**Application → Host → HBA/NIC → Fabric/Network → Storage Front End → Controller → Backend**

and, when replicated:

**Storage → Replication Interface → WAN → Remote Storage**

The narrowest sustained path can become the effective ceiling.

Nominal link speed is not equal to usable application throughput.

Account for protocol overhead, redundancy and failure-state path loss.

---

## 15. Oversubscription

Oversubscription is not automatically wrong.

It is acceptable when based on realistic concurrency and SLA.

Calculate or assess:

- aggregate host-facing bandwidth
- switch uplinks/ISLs
- storage-facing bandwidth
- expected concurrency
- failure-state links
- burst behavior

Do not require 1:1 line-rate everywhere unless the workload/SLA requires it.

---

## 16. Capacity versus Performance Growth

Capacity and performance may grow differently.

Examples:

- archive: capacity rises rapidly, IOPS slowly
- VDI: user count may drive IOPS faster than capacity
- database: transaction growth may drive IOPS/latency while dataset growth is modest
- AI: model/data/checkpoint throughput can dominate capacity

Model them separately.

---

## 17. Snapshot Capacity Model

Snapshot consumption depends primarily on changed data and retention, not simply source volume.

Conceptual inputs:

- protected capacity
- change rate
- snapshot frequency
- retention
- overwrite/delete behavior
- implementation efficiency
- immutable retention

Do not use a universal “10% snapshot reserve.”

Use scenarios when change-rate evidence is missing.

---

## 18. Backup Capacity Model

Minimum inputs:

- protected data
- daily change rate
- retention
- full/incremental strategy
- GFS
- compression/dedupe
- immutability
- backup copies
- growth
- repository filesystem/object overhead
- recovery requirements

Backup sizing is a separate model from production storage sizing.

---

## 19. Replication Capacity & Performance

Model at least:

- replicated dataset
- sustained write/change rate
- burst write rate
- RPO
- RTT
- available bandwidth
- journal/log requirements
- resync after outage
- initial synchronization
- failback

A link that handles normal replication may fail during resynchronization.

---

## 20. Restore Engineering

For physical data movement:

`Minimum Payload Rate = Data Size / Recovery Time`

Example:

500 TB in 15 minutes requires roughly multi-terabit-per-second payload movement.

Before rejecting/accepting such a requirement, clarify whether “restore” means:

- snapshot rollback
- instant recovery
- metadata switch
- clone presentation
- full physical copy
- application restart
- complete business service recovery

Recovery terminology matters.

---

## 21. Benchmark Discipline

A benchmark is meaningful only with context.

Record:

- block size
- R/W
- random/sequential
- dataset/working set
- cache state
- queue depth
- concurrency
- duration
- data reduction
- snapshot/replication state
- controller count
- failure state
- latency percentile

Never compare two headline IOPS numbers without checking conditions.

---

## 22. Datasheet Maximums

Vendor maximums can establish a product-class envelope.

They do not prove:

- customer workload performance
- failure-state performance
- mixed-workload behavior
- latency SLA
- exact offered configuration performance

Correct usage:

“Published maximum indicates the platform belongs in the required performance class; final sizing requires workload-specific validation.”

Incorrect usage:

“Customer needs 200K IOPS and datasheet says 4M, therefore 20× headroom.”

---

## 23. Scenario Analysis

When evidence is incomplete, use explicit scenarios.

Example:

- Scenario A: 10% CAGR
- Scenario B: 15% CAGR
- Scenario C: 20% CAGR

Label:

**SCENARIO ONLY — NOT A RECOMMENDED BOM**

Scenario analysis is preferable to hiding assumptions.

---

## 24. Sensitivity Analysis

Identify which variable most changes the design.

Common high-impact variables:

- CAGR
- data reduction
- failure-state SLA
- retention
- change rate
- CPU generation uplift
- SQL/Oracle licensing
- replication mode
- RTT
- peak write bandwidth

A design should prioritize validating high-sensitivity assumptions first.

---

## 25. Constraint Identification

For every sizing exercise identify the likely constraint:

- capacity-bound
- IOPS-bound
- throughput-bound
- latency-bound
- memory-bound
- CPU-bound
- port/network-bound
- licensing-bound
- rack/power-bound
- recovery-time-bound

Multiple constraints may coexist.

---

## 26. Telemetry Time Window

Short samples can miss:

- month-end
- backup windows
- batch processing
- seasonal peaks
- patching
- reporting
- quarter/year-end
- failover events

Prefer enough history to capture business cycles.

When only short telemetry exists, lower confidence rather than pretending it is representative.

---

## 27. Unit Discipline

Always preserve units.

Common errors:

- GB/s vs Gbps
- TB vs TiB
- MB/s vs MiB/s
- IOPS without block size
- latency ms vs µs
- decimal/binary capacity

Approximation is acceptable if clearly labeled.

---

## 28. Rounding Discipline

Avoid false precision.

If inputs are approximate, output should not imply engineering certainty.

Prefer:

“~1.10 PB required usable under this scenario”

over:

“1,101.462 TB exactly required”

unless exact arithmetic is needed for a later validated configuration.

---

## 29. Budgetary versus Physical Sizing

Budgetary sizing can use:

- required usable
- approximate performance class
- node/controller class
- memory class
- port class

Physical sizing requires:

- exact supported media
- protection geometry
- overhead
- adapter population
- chassis limits
- firmware/software
- licensing
- HCL
- vendor configuration rules

Do not cross this boundary silently.

---

## 30. Acceptance Criteria Template

A strong performance acceptance criterion should state:

- workload profile
- dataset
- duration
- IOPS
- throughput
- latency percentile
- snapshots/replication state
- failure state
- measurement point

Example pattern:

“System shall sustain the agreed mixed workload for 60 minutes at ≥X IOPS and ≥Y GB/s while p99 host-observed latency remains ≤Z ms, with replication enabled and one defined component failure active.”

The actual values must come from requirements, not from this template.

---

## 31. Capacity Acceptance Template

Specify:

- net production usable
- protection included
- system overhead treatment
- snapshot reserve treatment
- data reduction excluded/included
- TB/TiB
- growth horizon

Example pattern:

“Provide ≥X TB net production usable after required protection and system overhead, excluding data-reduction benefit and excluding Y TB snapshot/recovery reserve.”

---

## 32. Failure-State Acceptance

Ask whether requirements apply during:

- controller failure
- node failure
- drive/media failure
- fabric failure
- link failure
- maintenance
- site failure

If the customer says “no performance loss,” define measurable tolerance. Absolute zero degradation may be physically unrealistic or commercially excessive.

---

## 33. Performance Headroom

Headroom should be tied to a risk.

Possible model:

`Headroom Ratio = Validated Capacity / Required Peak`

But only when numerator and denominator use comparable workload conditions.

Do not compute “20× headroom” from unrelated benchmark conditions.

---

## 34. Bottleneck Reasoning

ARCHON should ask:

1. What resource saturates first?
2. What happens to latency at saturation?
3. Does failure move the bottleneck?
4. Does replication create another bottleneck?
5. Does growth change the dominant constraint?
6. Does licensing prevent the technically obvious scaling path?

The best design is not always the one with the highest maximum metric.

---

## 35. Compute Consolidation Trap

Reducing server count can:

- increase per-host failure blast radius
- increase RAM/node requirement
- increase NUMA pressure
- alter per-core licensing
- increase network/storage concentration
- make N+1 harder

Always evaluate both normal and surviving-node state.

---

## 36. Storage Consolidation Trap

Consolidating multiple arrays can:

- improve utilization
- simplify management
- reduce footprint

but also:

- increase blast radius
- combine incompatible SLA classes
- create migration risk
- increase port/fabric concentration
- complicate maintenance windows

Capacity efficiency alone is not enough.

---

## 37. Confidence Model

### LOW
Critical telemetry missing. Use conceptual/scenario analysis.

### MEDIUM
Enough evidence for budgetary sizing, but material normalization/vendor validation remains.

### HIGH
Workload, growth, failure state and implementation constraints are sufficiently validated for the stated decision.

Confidence applies to the conclusion, not the entire project.

---

## 38. Output Discipline

A sizing answer should separate:

### Facts
Customer/vendor evidence.

### Assumptions
Temporary values.

### Calculations
Transparent formulas.

### Results
Derived requirements.

### Risks
What can invalidate the result.

### Validation
What evidence is needed next.

This makes the sizing auditable.

---

## 39. Anti-Patterns

Challenge these immediately:

- “Average is only 30%, so we have plenty.”
- “There are only 300 VMs.”
- “The array can do 4M IOPS.”
- “We have 40 Gbps replication, so it is enough.”
- “3:1 reduction is guaranteed everywhere.”
- “N+1 means performance is protected.”
- “Eight nodes have 8 TB RAM, therefore 7 TB survives N+1.”
- “500 TB restore in 15 minutes” without defining restore.
- “20% headroom” without defining whether added or free.
- applying one CAGR to all resources.
- comparing old/new CPU cores 1:1.
- turning a scenario into an orderable BOM.

---

## 40. Escalation / TBD Conditions

Keep sizing provisional when:

- telemetry window is insufficient
- peak definition is unclear
- workload block size is missing
- growth meaning is ambiguous
- failure-state SLA is unknown
- new CPU performance normalization is absent
- reduction ratio is unvalidated
- replication write rate/RTT is unknown
- backup retention/change rate is unknown
- exact vendor configuration is not validated

**False precision is a defect. TBD is a valid engineering state.**

---

## 41. Final Principle

ARCHON should optimize for **defensible capacity and performance engineering**, not for producing a number quickly.

The correct sequence is:

**Measure → Understand → Normalize → Forecast → Stress → Fail → Validate → Configure**

Only after this sequence has enough evidence should a mathematical sizing result become a physical, orderable infrastructure configuration.


<!-- END SOURCE: 32-CAPACITY-PERFORMANCE-ENGINEERING.md -->


---

<!-- SOURCE: 33-HIGH-AVAILABILITY-FAILURE-DOMAINS.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** High Availability & Failure Domains  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for architecture principles; MEDIUM/HIGH for vendor-specific HA limits and behaviors  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Enterprise infrastructure HA, redundancy, failure domains, quorum, N+1/N+2, degraded-state behavior, maintenance, site resilience, blast radius and validation

# 33 — HIGH AVAILABILITY & FAILURE DOMAINS

## 1. Purpose

This pack teaches ARCHON to evaluate availability as an end-to-end system property rather than a collection of redundant components.

Core principle:

**Redundancy is not availability. Availability exists only when the complete service can continue or recover within its agreed SLA after defined failures.**

Architecture sequence:

**Business Service → SLA → Failure Scenarios → Failure Domains → Surviving Resources → Dependency Analysis → Recovery Mechanism → Operational Procedure → Validation**

---

## 2. Availability Vocabulary

### Availability
The ability of a service to remain accessible and usable as required.

### High Availability
Architecture and operational mechanisms designed to reduce service interruption caused by component or system failures.

### Fault Tolerance
Ability to continue operation despite defined failures, often with little or no interruption.

### Disaster Recovery
Recovery of service/data after a larger disruption, commonly involving another site or failure domain.

### Resilience
Ability to withstand, adapt to and recover from failures or disruptions.

### Redundancy
Duplication of components or paths.

Redundancy can contribute to availability but does not prove it.

---

## 3. RPO, RTO and Availability

### RPO
Maximum acceptable data-loss window.

### RTO
Maximum acceptable time to restore the required service.

### Availability SLA
Expected service accessibility over time or under defined conditions.

These are related but not interchangeable.

Examples:

- RPO=0 does not mean RTO=0.
- Synchronous replication does not automatically mean automatic application failover.
- 99.99% array availability does not prove 99.99% application availability.
- A second site does not automatically satisfy DR requirements.

---

## 4. Failure Domain

A failure domain is a boundary within which a single event can affect multiple components.

Potential domains include:

- drive/media
- enclosure/shelf
- controller
- node
- chassis
- HBA/NIC
- switch
- fabric
- network path
- PDU
- rack
- power feed
- room
- datacenter
- metro/site
- management plane
- identity service
- DNS
- application cluster
- storage system
- backup repository
- administrative/security domain

ARCHON should identify both obvious and hidden shared dependencies.

---

## 5. Hidden Shared Dependencies

Two components may appear redundant while sharing:

- same PDU
- same switch
- same cable route
- same rack
- same management service
- same authentication service
- same firmware defect
- same storage array
- same network uplink
- same replication link
- same administrator/security credentials

This creates correlated failure risk.

Physical duplication without independence can be false redundancy.

---

## 6. Single Point of Failure

A SPOF exists when failure of one component/dependency can violate the required service level.

Do not limit SPOF review to hardware.

Examples:

- one SAN fabric
- one witness
- one DNS service
- one backup credential domain
- one management server required for recovery
- one WAN circuit
- one application dependency
- one encryption key manager

---

## 7. N, N+1 and N+2

### N
Resources required to meet the defined workload/SLA.

### N+1
Enough additional capacity to tolerate one defined resource failure.

### N+2
Enough additional capacity to tolerate two defined failures, depending on architecture and failure independence.

Important:

**N+1 is meaningless unless “N” is defined under the relevant SLA.**

If four nodes can run the workload normally but three cannot meet the critical SLA, a four-node cluster is not truly N+1 for that SLA.

---

## 8. Capacity N+1 versus Performance N+1

A cluster may have enough surviving capacity but insufficient surviving performance.

Evaluate separately:

- CPU
- RAM
- storage capacity
- storage IOPS
- storage throughput
- network bandwidth
- FC bandwidth
- GPU resources
- application concurrency

Example:

8 nodes × 1 TB = 8 TB physical RAM.

After one node failure, only 7 TB remains before hypervisor/system overhead.

This does not prove that a 7 TB workload can safely run.

---

## 9. Failure-State Performance

For every material failure scenario ask:

1. What resource is lost?
2. What workload moves?
3. What surviving resource absorbs it?
4. What new bottleneck appears?
5. Does latency increase?
6. Does oversubscription change?
7. Does licensing permit the movement?
8. Does the application automatically rebalance?
9. Does the SLA still apply?
10. How long can the degraded state persist?

Do not assume linear redistribution.

---

## 10. Controller Failure

For storage evaluate:

- controller ownership
- cache mirroring/protection
- path failover
- surviving front-end ports
- surviving backend bandwidth
- volume ownership movement
- failover duration
- host timeout behavior
- performance after failover
- failback behavior
- maintenance equivalence

“Dual controller” is not enough.

---

## 11. Fabric Failure

Dual-fabric architecture should evaluate independence.

Check:

- host HBA placement
- switch independence
- storage port distribution
- zoning
- ISLs
- power
- cable routing
- multipathing
- surviving bandwidth

If loss of one fabric removes 50% of paths, verify whether the remaining fabric can carry failure-state demand.

---

## 12. Network Failure

For Ethernet-based storage, replication, HCI and application traffic evaluate:

- NIC failure
- switch failure
- uplink failure
- MLAG/vPC/stack dependencies
- VLAN/VRF dependencies
- routing
- MTU
- congestion
- QoS
- surviving bandwidth
- physical route diversity

Logical redundancy can still share a physical failure domain.

---

## 13. Power Failure

Validate:

- dual PSU
- separate power feeds
- independent PDU
- rack power budget
- UPS/generator domains
- site power design

Two PSUs connected to the same PDU are not independent power redundancy.

---

## 14. Rack Failure

Where required, distribute critical resources across racks.

Consider:

- compute nodes
- storage controllers/nodes
- switches
- PDUs
- network paths
- quorum/witness

Rack-level resilience can conflict with latency/cabling/architecture constraints; validate rather than assume.

---

## 15. Site Failure

A second datacenter is only useful if dependencies are sufficiently independent.

Evaluate:

- compute
- storage
- network
- DNS
- identity
- application services
- load balancers
- security controls
- backup
- management
- monitoring
- key management
- operational access

“Two DC” does not equal “site-resilient application.”

---

## 16. Active/Active versus Active/Passive

These terms are ambiguous.

Clarify what is active:

- storage controllers?
- storage systems?
- compute?
- application?
- database?
- site?
- network?
- workload writes?

Two sites may both run workloads while an individual application remains active/passive.

Always define the layer.

---

## 17. Quorum

Distributed systems often need quorum to avoid conflicting ownership or split-brain.

Understand:

- voting members
- majority requirements
- witness role
- failure scenarios
- network partition behavior
- witness reachability
- witness failure domain

A witness should generally not share the same failure domain as the components it is intended to arbitrate.

Exact quorum rules are product-specific and must be validated.

---

## 18. Witness

A witness can provide arbitration but is not automatically a data replica.

Do not describe a witness as a third storage copy unless the technology explicitly stores protected data.

Validate:

- deployment location
- connectivity
- latency
- security
- availability
- supported topology
- failure behavior

---

## 19. Split Brain

Split brain occurs when separated components can incorrectly believe they own the same service/resource.

Mitigations may include:

- quorum
- witness
- fencing
- STONITH
- storage reservations
- consensus protocols
- application-specific arbitration

ARCHON should ask how ownership is protected during network partition, not only hardware failure.

---

## 20. Failure versus Partition

A component can be alive but unreachable.

Network partition behavior can be more complex than simple component failure.

Test scenarios should distinguish:

- node powered off
- link down
- packet loss
- asymmetric reachability
- high latency
- partial site isolation

---

## 21. Planned Maintenance

HA must support operations, not only disasters.

Consider:

- firmware upgrades
- OS/hypervisor updates
- controller upgrades
- switch maintenance
- storage code upgrades
- hardware replacement
- migration

A design that survives an unexpected failure but cannot tolerate planned maintenance without SLA violation is operationally weak.

---

## 22. Maintenance + Failure

One of the most important scenarios:

**A component is already offline for maintenance when another component fails.**

This can turn N+1 into N or worse.

Where business criticality requires it, evaluate maintenance-state resilience separately.

---

## 23. Correlated Failures

Not all failures are independent.

Examples:

- firmware defect affects both controllers
- configuration error affects all switches
- ransomware compromises production and connected backups
- power event affects entire rack
- operator error deletes replicated data
- software bug propagates across cluster

HA design must consider common-mode failure, not only component MTBF.

---

## 24. Blast Radius

Blast radius is the scope of impact caused by a failure or mistake.

Consolidation can increase blast radius.

Evaluate:

- VMs per host
- workloads per storage array
- tenants per cluster
- datastores/volumes per failure group
- backup jobs per repository
- sites dependent on one management plane

High utilization may be economically efficient but operationally concentrated.

---

## 25. Isolation Boundaries

Potential isolation strategies:

- separate clusters
- separate storage pools
- separate fabrics
- separate admin roles
- separate credentials
- separate backup domains
- separate sites
- separate replication groups

Isolation should be driven by risk/SLA/security, not arbitrary product boundaries.

---

## 26. Recovery Groups

Group workloads according to:

- application dependency
- consistency
- RPO
- RTO
- failover order
- business service

Avoid designing replication/recovery solely around storage volume convenience.

A multi-tier application may require coordinated recovery across several volumes and systems.

---

## 27. Dependency Mapping

For critical services map:

**User → DNS/LB → Application → Database → Compute → Network → Storage → Replication/Backup → Identity/Management**

Any dependency can dominate availability.

Storage HA alone cannot compensate for an unavailable database listener, DNS service or network path.

---

## 28. Failure-State Capacity

If surviving site must carry all workloads:

`Failure-State Demand = Workload moved from failed site + surviving site's workload`

Do not assume all workloads are equally critical.

If only critical workloads must survive, quantify the critical percentage.

Example:

“100% of critical workloads” is not equivalent to “100% of total workload.”

---

## 29. Failure-State Growth

A five-year architecture must test failure-state capacity at the future horizon, not only today.

Sequence:

1. project workload to target year
2. determine critical/failover scope
3. calculate surviving resources
4. apply operational/system reserves
5. validate performance and capacity

---

## 30. Replication and HA

Replication provides data movement/protection.

HA additionally requires:

- ownership
- orchestration
- host/application access
- network reachability
- consistency
- failover
- failback
- quorum/witness where applicable

Do not equate replication capability with HA.

---

## 31. RPO=0

RPO=0 generally requires no acknowledged committed data to be lost under the defined failure model.

Validate:

- synchronous semantics
- failure scope
- application consistency
- network behavior
- quorum
- acknowledgement model

RPO=0 does not promise zero downtime.

---

## 32. RTO=0

Absolute RTO=0 is an unusually strict requirement and often not physically/operationally realistic as a blanket statement.

Clarify whether the customer means:

- transparent failover
- seconds
- no manual intervention
- no application restart
- no transaction interruption

Turn vague absolutes into measurable acceptance criteria.

---

## 33. Backup Failure Domain

Backup should ideally avoid sharing all failure/security domains with production.

Evaluate:

- storage independence
- credential independence
- administrative separation
- immutability
- offsite copy
- offline/air-gap copy
- recovery access
- ransomware propagation risk

A backup copy reachable with the same compromised administrative plane may not provide the intended resilience.

---

## 34. Cyber Failure Domain

Cyber incidents cross physical redundancy boundaries.

Two replicated arrays can both contain encrypted/corrupted data.

Cyber resilience needs:

- protected recovery points
- isolation
- detection
- retention
- clean recovery
- administrative separation

Physical HA and cyber resilience are different design dimensions.

---

## 35. HA and Licensing

Failover can create licensing consequences.

Examples may include:

- per-core software
- database licensing
- virtualization subscriptions
- standby/DR rights
- application licenses

Do not assume a technically valid failover topology is commercially licensed.

Licensing terms are volatile and require current validation.

---

## 36. HA and Performance Headroom

Do not add arbitrary headroom twice.

Example:

If N+1 already requires one full node of spare capacity, understand whether an additional 20% performance headroom is also required and why.

Document each reserve:

- failure reserve
- growth reserve
- burst reserve
- operational reserve

Avoid hidden double-counting.

---

## 37. Availability Percentages

Annual downtime equivalents can be useful context, but do not treat theoretical “nines” as architecture proof.

Availability depends on:

- failure rates
- repair time
- maintenance
- software
- operations
- dependencies
- change management

Vendor availability claims may have specific terms and exclusions.

Use them as evidence only within their documented scope.

---

## 38. Failure Matrix

For important designs create a matrix:

| Failure | Service Impact | Surviving Resources | Automatic? | RPO | RTO | Performance Impact | Validation |
|---|---|---|---|---|---|---|---|
| Single drive | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Controller/node | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Fabric/switch | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Host | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| WAN | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Site | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

Do not fill unknown cells with optimistic assumptions.

---

## 39. Failure Injection / Testing

Where practical validate:

- path loss
- HBA/NIC failure
- switch/fabric loss
- controller/node failure
- drive/media failure
- WAN loss
- witness loss
- site isolation
- maintenance operation
- failover
- failback

Measure:

- interruption
- latency
- throughput
- recovery time
- application errors
- data consistency

A failover that “works” but violates application SLA is not a full PASS.

---

## 40. Acceptance Criteria

A strong HA criterion defines:

- failure being tested
- workload level
- permitted interruption
- permitted performance degradation
- RPO
- RTO
- automation/manual steps
- measurement point
- recovery/failback requirement

Weak:

“System shall be highly available.”

Strong pattern:

“During loss of one defined controller/node, service shall continue within the agreed interruption and p99 latency thresholds under the specified workload.”

Values must come from customer requirements.

---

## 41. Anti-Patterns

Challenge:

- dual PSU on one PDU = power HA
- two switches in one logical/physical failure domain = independent fabrics
- dual controller = DR
- replication = HA
- RPO=0 = RTO=0
- N+1 = no performance impact
- active/active without defining layer
- two DC = site-resilient service
- witness = third data copy
- snapshots = cyber isolation
- identical redundant components = protection from common-mode software defect
- normal-state benchmark = failure-state SLA
- capacity survives failure, therefore performance survives
- “no downtime ever” without measurable scope

---

## 42. Architecture Challenge Questions

1. What is the largest credible failure domain?
2. Which two “redundant” components share a hidden dependency?
3. What happens if failure occurs during maintenance?
4. What survives after one controller/node/fabric/site is lost?
5. Can surviving resources meet critical SLA?
6. Which workloads must survive and which may stop?
7. What is automatic and what requires an operator?
8. Where is quorum/witness and what happens if it is lost?
9. How is split brain prevented?
10. Does failback have a separate outage/risk?
11. Can a cyber incident bypass physical redundancy?
12. Are licenses valid after failover?
13. Has the failure been tested under realistic workload?
14. What happens at year 5, not only day 1?

---

## 43. Confidence

### LOW
Failure domains or SLAs are largely unknown.

### MEDIUM
Topology and major failure behavior are understood, but product-specific or operational validation remains.

### HIGH
Failure scenarios, surviving resources, recovery procedures, dependencies and implementation behavior are validated for the decision being made.

A topology diagram alone does not justify HIGH confidence.

---

## 44. Escalation / TBD

Keep HA claims TBD when:

- active/active meaning is ambiguous
- RPO/RTO is missing
- critical workload percentage is unknown
- failure-state performance is unknown
- witness/quorum behavior is unvalidated
- fabric/power independence is unknown
- failover/failback procedure is unknown
- licensing rights are unknown
- vendor-specific HA limits are not current
- application dependencies are unmapped

---

## 45. Final Principle

ARCHON should never ask only:

**“Is it redundant?”**

It should ask:

**“Which failure are we protecting against, what remains after that failure, can the surviving architecture still meet the business SLA, and have we validated that behavior?”**

The engineering sequence is:

**Define Failure → Identify Domain → Remove Shared Dependencies → Calculate Surviving Capacity → Validate Performance → Validate Recovery → Test → Document Residual Risk**

That is the difference between redundant infrastructure and a defensible high-availability architecture.


<!-- END SOURCE: 33-HIGH-AVAILABILITY-FAILURE-DOMAINS.md -->

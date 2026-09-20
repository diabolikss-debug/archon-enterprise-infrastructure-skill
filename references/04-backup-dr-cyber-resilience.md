# 04 BACKUP DR CYBER RESILIENCE

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.


---

<!-- SOURCE: 24-BACKUP-RECOVERY-ARCHITECTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Backup & Recovery Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for architecture principles; MEDIUM/HIGH for product-specific features, licensing, compatibility and repository limits  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Enterprise backup, recovery, retention, repository sizing, immutability, 3-2-1-1-0, air-gap, restore engineering, instant recovery, GFS, tape/object/disk tiers and operational validation

# 24 — BACKUP & RECOVERY ARCHITECTURE

## 1. Purpose

This pack teaches ARCHON to design backup around recovery outcomes rather than around backup-job success.

Core principle:

**A backup is valuable only if the required data and business service can be recovered within the required time, from a trustworthy recovery point, after the failures and attacks the architecture is intended to survive.**

Reasoning sequence:

**Business Service → Data Scope → RPO/RTO → Change Rate → Retention → Backup Method → Repository Architecture → Immutability/Isolation → Copy Policy → Recovery Method → Restore Performance → Validation**

---

## 2. Backup Is Not Snapshot

A storage snapshot can provide fast local recovery, but may share:

- storage failure domain
- administrative plane
- credentials
- site
- software defect
- cyber compromise

Snapshots can be part of a protection strategy, but do not automatically replace backup.

---

## 3. Backup Is Not Replication

Replication can copy:

- deletion
- corruption
- ransomware encryption
- application error

Backup should provide historical recovery points and ideally independent failure/security domains.

Replication protects availability and data location. Backup protects recoverability over time. They may complement each other.

---

## 4. Recovery First

Start discovery with:

- What must be recovered?
- From which failure?
- How much data loss is acceptable?
- How quickly must it be usable?
- How far back must recovery points exist?
- Must recovery be clean/validated?
- What dependencies must recover with it?

Do not begin with “How many TB of backup storage?”

---

## 5. RPO

Backup RPO defines acceptable data-loss exposure.

Inputs:

- backup frequency
- snapshot frequency
- replication
- log backup/CDP where applicable
- application behavior

A nightly backup does not satisfy a 15-minute RPO unless another protection mechanism closes the gap.

---

## 6. RTO

Recovery time includes:

- locating recovery point
- authorization
- repository access
- data movement
- VM/application startup
- database recovery
- network/DNS
- validation

Repository throughput alone does not define RTO.

---

## 7. Retention

Retention should be tied to:

- operational recovery
- compliance
- legal requirements
- ransomware dwell time
- audit
- business policy

Clarify:

- daily
- weekly
- monthly
- yearly
- GFS
- immutable retention
- offsite retention

Long retention can dominate repository sizing.

---

## 8. Change Rate

Daily change rate is one of the most important sizing inputs.

Example conceptual model:

`Daily Changed Data = Protected Data × Daily Change Rate`

But actual backup storage depends on:

- backup method
- dedupe/compression
- synthetic/full behavior
- retention
- metadata
- immutability
- GFS
- growth

Do not infer change rate from total production capacity.

---

## 9. Full and Incremental

Possible methods include:

- active full
- synthetic full
- forward incremental
- reverse incremental
- forever-forward incremental
- snapshot-based backup
- CDP/log-based protection

Architecture impact includes:

- source load
- repository I/O
- network traffic
- backup window
- retention chain
- recovery behavior

Exact implementation is product-specific.

---

## 10. GFS

Grandfather-Father-Son retention typically preserves periodic long-term restore points.

GFS can significantly increase capacity because retained full recovery points may persist beyond normal short-term chains.

Always include GFS explicitly in sizing.

---

## 11. Repository Sizing Inputs

Minimum inputs:

- protected data
- current consumed data
- growth
- daily change rate
- retention
- backup method
- full frequency
- GFS
- compression
- deduplication
- immutability
- copy jobs
- offsite copies
- repository overhead
- free-space policy

Without these, backup capacity is scenario-only.

---

## 12. Data Reduction

Backup compression/deduplication depends on:

- workload
- source compression
- encryption
- backup format
- repository technology
- cross-job/global dedupe behavior

Do not assume a generic reduction ratio.

Show physical requirement separately from assumed logical/effective savings.

---

## 13. Backup Window

A backup must complete within the operational window.

Approximation:

`Required Ingest Rate = Data To Protect / Backup Window`

But include:

- concurrency
- source read rate
- network
- proxies/media servers
- repository write rate
- synthetic operations
- competing workloads

---

## 14. Restore Throughput

Approximation:

`Required Restore Payload Rate = Data To Restore / RTO`

Then validate the complete path:

**Repository → Network/SAN → Proxy/Media Server → Production Storage → Compute/Application**

The slowest stage can determine recovery time.

---

## 15. Instant Recovery

Instant Recovery may allow workloads to run directly or indirectly from backup storage before full data migration completes.

It can dramatically reduce service RTO but creates temporary performance requirements on the backup platform.

Validate:

- supported workload
- repository performance
- concurrency
- network
- write handling
- migration back to production
- duration
- licensing

Instant Recovery is not the same as restoring all bytes.

---

## 16. Full Restore versus Logical Recovery

Clarify whether “restore 500 TB” means:

- entire physical copy
- VM restore
- instant recovery
- file restore
- database point-in-time recovery
- snapshot rollback
- volume recovery
- application service restoration

These have radically different bandwidth and RTO implications.

---

## 17. 3-2-1

Conceptual principle:

- 3 copies of data
- 2 different media/storage types
- 1 offsite copy

Modern cyber-resilience strategies often extend this.

---

## 18. 3-2-1-1-0

Common conceptual interpretation:

- 3 copies
- 2 media/storage types
- 1 offsite
- 1 offline/air-gapped/immutable copy
- 0 recovery errors after verification

Treat this as a resilience principle, not a substitute for customer-specific risk analysis.

---

## 19. Immutability

Immutability protects recovery points from alteration/deletion for a defined period.

Validate:

- enforcement mechanism
- retention lock
- administrator override behavior
- clock/time dependencies
- credential model
- supported storage
- operational exceptions
- legal hold where relevant

“Immutable” should not be accepted as a vague marketing label.

---

## 20. Air Gap

Air gap may be:

- physical
- logical
- operational
- media-based

Tape can provide strong physical/offline separation when removed/offline according to procedure.

Object/disk repositories can provide logical isolation/immutability.

Air-gap effectiveness depends on operational process.

---

## 21. Hardened Repository

A hardened repository aims to reduce compromise risk through:

- limited attack surface
- strong authentication
- role separation
- immutability
- restricted management
- controlled updates
- network segmentation

Exact implementation is product-specific.

Do not assume “Linux repository” is automatically hardened.

---

## 22. Object Storage

Object storage can be useful for:

- capacity tier
- archive
- immutable/object-lock copies
- cloud/offsite protection

Evaluate:

- API compatibility
- object lock
- retention
- performance
- egress
- network
- restore latency
- object count
- lifecycle policies
- failure domain

---

## 23. Tape

Tape remains relevant for:

- long-term retention
- offline copies
- air-gap
- large-scale archive
- cyber resilience

Evaluate:

- native capacity
- drive generation
- drive count
- cartridge count
- ingest/restore throughput
- library slots
- cleaning/media
- encryption
- WORM
- offsite handling
- retention
- operational process

Do not size tape using compressed marketing capacity unless justified by workload and requirement.

---

## 24. Disk Repository

Disk repositories offer fast backup/restore but must be sized for:

- ingest
- random/sequential behavior
- synthetic operations
- concurrent restores
- immutability
- capacity growth
- failure protection

A capacity-only disk repository can fail recovery performance.

---

## 25. Repository Tiering

A design may combine:

- performance tier
- capacity tier
- object tier
- archive/tape tier

Use tiering based on:

- RTO
- retention
- cost
- immutability
- locality
- recovery frequency

Not every restore point requires the fastest storage.

---

## 26. Failure Domains

Separate where required:

- production storage
- primary backup repository
- secondary copy
- offsite copy
- identity/admin credentials
- management plane

A backup stored on the same array as production may be useful operationally but shares major failure domains.

---

## 27. Administrative Separation

Cyber resilience may require backup administrators/credentials to be separated from production administration.

Evaluate:

- RBAC
- MFA
- privileged access
- service accounts
- credential storage
- break-glass procedures
- audit logs

Technical immutability can be weakened by poor identity design.

---

## 28. Encryption

Consider:

- in-flight encryption
- at-rest encryption
- key management
- key recovery
- performance
- compliance

Encrypted backup that cannot recover its keys is not recoverable.

Key management must be part of DR.

---

## 29. Application-Aware Backup

Crash-consistent backup may not be sufficient for:

- databases
- directory services
- transactional applications

Evaluate:

- application quiescing
- VSS or equivalent mechanisms
- transaction logs
- database-native integration
- point-in-time recovery

Application consistency is workload-specific.

---

## 30. Database Protection

For databases consider:

- full backup
- incremental/differential
- transaction logs
- PITR
- consistency
- recovery sequence
- database size
- log generation rate
- licensing/integration

A VM-level backup alone may not meet database RPO/RTO.

---

## 31. Virtualization

For VMware/Hyper-V consider:

- snapshot interaction
- Changed Block Tracking or equivalent
- proxy architecture
- transport mode
- datastore load
- concurrent jobs
- instant recovery
- granular restore
- application-aware processing

Exact capabilities are product/version-specific.

---

## 32. Backup Concurrency

Increasing job concurrency can shorten windows but can saturate:

- source storage
- proxies
- network
- repository
- CPU
- metadata services

Tune concurrency based on end-to-end telemetry.

---

## 33. Source Impact

Backup can affect production.

Measure:

- snapshot stun/quiesce
- source reads
- datastore latency
- CPU
- network
- application response

Backup architecture must respect production SLA.

---

## 34. Recovery Concurrency

Disaster recovery may require many workloads simultaneously.

Repository design should consider:

- concurrent VM starts
- parallel restores
- instant recovery load
- database recovery
- network
- production target write rate

A repository optimized only for backup ingest may underperform during mass recovery.

---

## 35. Recovery Prioritization

Classify workloads.

Example:

- Tier 0: identity/core infrastructure
- Tier 1: critical business services
- Tier 2: important services
- Tier 3: low-priority/archive

Actual tier definitions and RPO/RTO must come from the customer.

Recovery order matters.

---

## 36. Clean Recovery

After cyber incident, fastest restore point may not be safe.

Clean recovery can require:

- selecting trusted point
- malware scanning
- isolated recovery
- credential reset
- validation
- application testing

Cyber RTO may be longer than infrastructure restore time.

---

## 37. Recovery Verification

“Backup completed successfully” does not prove recoverability.

Validation can include:

- automated verification
- test boot
- application checks
- checksum/integrity checks
- periodic restore tests
- isolated recovery tests

The “0” in 3-2-1-1-0 emphasizes verified recovery integrity.

---

## 38. Backup Monitoring

Monitor:

- job success
- SLA/RPO compliance
- repository capacity
- immutability state
- job duration
- throughput
- failed restore points
- copy/offsite status
- tape/media health
- recovery testing

Green backup jobs alone are insufficient.

---

## 39. Capacity Growth

Project:

- production growth
- retention growth
- change-rate growth
- new workloads
- GFS accumulation
- immutable retention
- secondary copies

Backup capacity can grow faster than production capacity.

---

## 40. Free Space

Repositories often require operational free space.

Reason depends on technology:

- merge/synthetic operations
- immutability
- filesystem behavior
- object operations
- expansion
- recovery staging

Use vendor-specific guidance for final configuration.

---

## 41. Backup Copy

A backup copy can improve resilience when placed in another failure/security domain.

Evaluate:

- schedule
- bandwidth
- retention
- immutability
- offsite location
- recovery accessibility

A second copy on the same compromised platform may not satisfy resilience goals.

---

## 42. Offsite

Offsite protection should consider:

- distance
- disaster correlation
- bandwidth
- legal/data residency
- recovery logistics
- security
- access during disaster

“Cloud” is not automatically an optimal offsite recovery target.

---

## 43. Ransomware Protection

A layered design may include:

**Prevent → Detect → Protect → Isolate → Recover → Validate**

Backup-specific controls can include:

- immutable copies
- offline copies
- MFA/RBAC
- isolated credentials
- anomaly detection
- retention
- clean-room recovery
- recovery verification

No single feature is a complete ransomware strategy.

---

## 44. Backup SLA

Define:

- RPO
- backup completion window
- retention
- recovery time
- recovery granularity
- number of concurrent recoveries
- cyber-recovery expectations

Do not confuse backup-job SLA with business recovery SLA.

---

## 45. Sizing Scenario

When data is incomplete, create explicit scenarios.

Example inputs:

- 500 TB protected
- 3% daily change
- 30-day retention
- weekly/monthly GFS
- 15% annual growth

But do not convert this into a recommended repository size until backup method, reduction, full behavior and overhead are known.

Label:

**SCENARIO ONLY — NOT A RECOMMENDED BOM**

---

## 46. Evidence Hierarchy

For exact product claims use:

1. current vendor documentation
2. current compatibility/HCL
3. current sizing/configuration tool
4. validated design/reference architecture
5. support policy
6. credible field evidence

Highly volatile:

- licensing
- repository limits
- supported OS/filesystem
- immutability implementation
- object compatibility
- tape drive/library support
- software version compatibility

---

## 47. Configuration Maturity

### Level 0 — Discovery
RPO/RTO, retention, change rate incomplete.

### Level 1 — Conceptual
Protection topology and copy strategy.

### Level 2 — Budgetary
Approximate repository/tier capacity and performance class.

### Level 3 — Validated Physical
Exact repository/storage/media/proxy/network configuration validated.

### Level 4 — Operationally Ready
Jobs, immutability, monitoring, restore tests, runbooks and recovery procedures validated.

A purchased backup repository is not automatically an operationally ready recovery system.

---

## 48. Acceptance Testing

Test:

- backup completion
- file restore
- VM restore
- application-aware restore
- database PITR
- instant recovery
- mass recovery
- immutable restore point
- offsite copy
- repository failure
- clean recovery where relevant

Measure actual RPO/RTO.

---

## 49. Anti-Patterns

Challenge:

- backup capacity = production TB × fixed ratio
- snapshot = backup
- replication = backup
- immutable = impossible to compromise
- tape compressed capacity = guaranteed
- nightly backup = any RPO
- backup job success = recoverability
- fast ingest = fast restore
- one repository = cyber resilience
- cloud = air gap
- Linux repository = hardened repository
- instant recovery = full restore
- production growth = backup growth
- repository capacity alone = backup architecture
- “we have backups” without restore testing

---

## 50. Architecture Challenge Questions

1. What business services must be recovered first?
2. What are RPO/RTO by workload?
3. What is the real daily change rate?
4. What retention/GFS is required?
5. Which recovery points must be immutable?
6. Which copy is outside the production security domain?
7. Can the repository meet mass-recovery throughput?
8. What happens if the backup server/admin credentials are compromised?
9. How is key recovery handled?
10. Are database logs/application consistency protected?
11. Can the offsite copy be accessed during a disaster?
12. How long would a full physical restore actually take?
13. Is Instant Recovery available and adequately sized?
14. When was the last successful restore test?
15. How is a clean recovery point selected after ransomware?
16. Which claims require current product evidence?

---

## 51. Final Principle

ARCHON should not ask:

**“How much backup storage do we need?”**

until it can first answer:

**“What must recover, from which failure, to which point in time, within how long, from which trustworthy copy, and at what scale?”**

Engineering sequence:

**Classify Data → Define RPO/RTO → Measure Change → Define Retention → Design Copies → Separate Failure Domains → Size Ingest → Size Restore → Add Immutability/Isolation → Validate Application Recovery → Test → Measure**

The objective is not successful backup.

The objective is **reliable, timely and trustworthy recovery**.


<!-- END SOURCE: 24-BACKUP-RECOVERY-ARCHITECTURE.md -->


---

<!-- SOURCE: 25-CYBER-RESILIENCE-ARCHITECTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Cyber Resilience Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for architecture principles; MEDIUM/HIGH for vendor-specific security, immutability, detection and integration capabilities  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Ransomware/destructive attack resilience across storage, backup, identity, management, isolation, detection, immutable recovery, clean recovery and operational validation

# 25 — CYBER RESILIENCE ARCHITECTURE

## 1. Purpose

This pack teaches ARCHON to treat cyber resilience as a recovery architecture spanning infrastructure, identity, backup, storage and operations.

Core principle:

**High availability protects service from failures. Cyber resilience protects the organization’s ability to recover trusted data and services after malicious or destructive change.**

Reasoning sequence:

**Identify Critical Services → Model Threat/Blast Radius → Prevent → Detect → Protect → Isolate → Respond → Recover → Validate → Improve**

No single immutable snapshot, backup appliance, storage feature or security product constitutes a complete cyber-resilience architecture.

---

## 2. Cyber Resilience versus HA and DR

### High Availability
Primarily addresses component/service failures.

### Disaster Recovery
Primarily addresses recovery from major disruptions such as site loss.

### Cyber Resilience
Addresses deliberate or destructive events that may propagate across otherwise healthy redundant systems.

Examples:

- ransomware
- destructive administrator compromise
- credential theft
- malicious deletion
- logical corruption
- backup-system compromise
- encryption-key compromise

A perfectly replicated corruption can satisfy HA while destroying recoverability.

---

## 3. Resilience Lifecycle

ARCHON should reason through:

**Prevent → Detect → Protect → Isolate → Respond → Recover → Validate**

### Prevent
Reduce likelihood and attack surface.

### Detect
Identify suspicious behavior early.

### Protect
Preserve recovery points and critical configuration.

### Isolate
Prevent compromise from reaching all recovery assets.

### Respond
Contain and assess the incident.

### Recover
Restore trusted services/data.

### Validate
Confirm recovered environment is clean, consistent and usable.

---

## 4. Threat Model

Before selecting technology, clarify threats.

Potential scenarios:

- ransomware encrypts production data
- privileged account deletes snapshots/backups
- attacker compromises backup server
- storage administrator credentials stolen
- malicious insider
- production and replicated copy corrupted
- identity platform unavailable
- key management compromised
- management network compromised
- destructive configuration change

The architecture should state which threats it is designed to survive.

---

## 5. Blast Radius

Ask what one compromised identity, system or management plane can control.

Map:

- production storage
- backup
- hypervisor
- SAN/network
- object storage
- tape/library
- cloud
- key management
- monitoring
- identity

Shared administration can create a cyber failure domain larger than the physical topology suggests.

---

## 6. Administrative Separation

Separate privileged roles where justified.

Potential boundaries:

- production infrastructure admins
- backup admins
- security admins
- storage admins
- recovery operators
- key-management admins

Use:

- least privilege
- RBAC
- MFA
- privileged access management
- separate service accounts
- break-glass controls
- audit

Do not assume separate usernames are sufficient if they share the same compromised identity authority.

---

## 7. Identity as a Recovery Dependency

Recovery may fail if identity services are unavailable or compromised.

Plan for:

- directory recovery
- MFA dependencies
- local/break-glass access
- privileged credentials
- credential rotation
- offline recovery procedures

Identity recovery can be Tier 0.

---

## 8. Immutability

Immutability protects data/recovery points from modification or deletion for a defined retention period.

Validate:

- enforcement layer
- administrator override
- retention lock
- time/clock dependency
- credential dependency
- supported operations
- expiration behavior
- legal hold where applicable

“Immutable” is not binary without understanding the threat model.

---

## 9. Logical versus Physical Isolation

### Logical Isolation
Examples:
- immutable repository
- object lock
- network segmentation
- isolated credentials

### Physical/Offline Isolation
Examples:
- exported/offline tape
- disconnected media/system
- controlled vaulting

Both can be useful.

Logical isolation is operationally convenient; physical separation can reduce online attack reachability.

---

## 10. Air Gap

An air gap should specify:

- what is disconnected
- when it is disconnected
- who can reconnect it
- how updates occur
- how recovery is performed
- how integrity is verified

A network-connected system marketed as “air-gapped” should be examined carefully.

---

## 11. Tape

Tape can provide strong offline separation when operational procedures actually remove or isolate media.

Cyber-resilience considerations:

- WORM
- encryption
- key custody
- media export
- vaulting
- catalog protection
- library credentials
- restore logistics
- periodic testing

Tape alone does not guarantee fast recovery.

---

## 12. Object Lock

Object storage may provide retention/lock mechanisms.

Validate:

- governance/compliance modes
- privileged deletion behavior
- retention policy
- versioning
- API compatibility
- backup software support
- lifecycle policy interaction

Exact semantics are product-specific.

---

## 13. Storage Snapshots

Storage snapshots can provide rapid recovery points.

Cyber value depends on:

- protection from deletion
- administrative separation
- retention
- replication behavior
- anomaly detection
- recovery workflow

Ordinary deletable snapshots should not be described as ransomware-proof.

---

## 14. Detection

Potential detection sources:

- storage I/O anomaly detection
- backup anomaly detection
- file entropy/change behavior
- SIEM
- endpoint security
- identity analytics
- network telemetry

Detection is probabilistic and context-dependent.

Do not claim a detection feature guarantees ransomware identification.

---

## 15. Detection versus Protection

Detection answers:

**“Something suspicious may be happening.”**

Protection answers:

**“Can trusted recovery points survive?”**

A system can detect ransomware yet still lose recovery assets if protection/isolation is weak.

Conversely, immutable copies can survive even if detection is delayed.

Use both where risk justifies it.

---

## 16. Recovery Point Selection

The newest restore point may already be contaminated.

Cyber recovery needs the ability to determine:

- when compromise began
- which recovery points are trustworthy
- whether credentials/configuration were already compromised
- whether malware persists

Retention should consider attacker dwell time, not only operational mistakes.

---

## 17. Clean Recovery

Clean recovery may involve:

1. incident containment
2. trusted recovery environment
3. credential reset
4. selecting candidate recovery point
5. malware/security validation
6. restoring infrastructure dependencies
7. restoring application/data
8. application validation
9. controlled reconnection
10. monitoring

Fast byte restoration alone does not equal cyber recovery.

---

## 18. Isolated Recovery Environment

A clean room / isolated recovery environment can help validate recovered systems before reconnecting them.

Consider:

- isolated network
- compute
- storage
- identity
- security tools
- malware scanning
- application validation
- data access controls

Exact implementation depends on scale and risk.

---

## 19. Recovery Dependencies

Critical recovery assets may include:

- backup catalog/database
- encryption keys
- storage configuration
- hypervisor configuration
- network configuration
- DNS
- directory services
- certificates
- application secrets
- runbooks

Protect metadata and configuration, not only business data.

---

## 20. Key Management

Encryption is valuable only if keys remain recoverable and protected.

Plan:

- key backup
- separation of duties
- HSM/KMS resilience
- DR access
- rotation
- revocation
- emergency procedures

Loss of keys can turn intact backup data into unrecoverable data.

---

## 21. Management Plane Security

Separate management from data access where appropriate.

Controls may include:

- management network segmentation
- MFA
- RBAC
- secure protocols
- restricted source networks
- logging
- privileged session controls
- configuration backups

A compromised management plane can bypass otherwise resilient data paths.

---

## 22. Monitoring and Audit

Collect relevant events from:

- storage
- backup
- hypervisor
- identity
- network
- security platforms

Useful events include:

- privileged login
- role changes
- snapshot deletion attempts
- immutability changes
- repository changes
- backup job changes
- replication changes
- mass deletion/encryption anomalies

Logs should be protected from the same compromise where possible.

---

## 23. SIEM Integration

SIEM can centralize and correlate events.

Validate:

- supported log format
- syslog/API
- event coverage
- timestamps
- retention
- identity context
- alert quality

Do not assume every administrative action is logged merely because SIEM integration exists.

Exact product logging capabilities are version-specific.

---

## 24. Backup Infrastructure Hardening

Protect:

- backup server
- proxies
- repositories
- service accounts
- management consoles
- credentials
- catalogs
- encryption keys

Backup systems are high-value attack targets.

---

## 25. Network Segmentation

Potential segmentation:

- production
- management
- backup
- replication
- recovery/clean room
- out-of-band management

Segmentation should reduce attack paths without making recovery operationally impossible.

---

## 26. Replication Risk

Replication can rapidly propagate destructive changes.

Mitigate with:

- historical recovery points
- immutable copies
- retention
- isolated backup
- anomaly detection
- recovery testing

Synchronous replication is not cyber protection by itself.

---

## 27. RPO in Cyber Incidents

Traditional RPO asks how much recent data can be lost.

Cyber recovery may require intentionally going back hours/days/weeks to a clean point.

Therefore distinguish:

- operational RPO
- clean-recovery-point age

They are different measures.

---

## 28. RTO in Cyber Incidents

Cyber RTO can include:

- forensic analysis
- containment
- credential reset
- clean-point selection
- malware validation
- restore
- application validation
- controlled reconnection

Cyber RTO may be longer than hardware/site DR RTO.

Do not promise DR RTO automatically applies to ransomware recovery.

---

## 29. Recovery Prioritization

Prioritize dependencies and business services.

Potential sequence:

1. recovery control plane
2. identity
3. network/security
4. backup/catalog/key services
5. core databases
6. critical applications
7. secondary workloads

Actual order must reflect customer dependencies.

---

## 30. Recovery at Scale

Mass recovery stresses:

- repository read performance
- network
- proxies/media servers
- production storage writes
- compute
- DNS/identity
- operations

A solution optimized for backup ingest may fail mass-recovery RTO.

---

## 31. Recovery Verification

Verify:

- data integrity
- application startup
- authentication
- dependencies
- security posture
- malware indicators
- business transactions

Recovery success is not merely “VM powered on.”

---

## 32. Testing

Cyber-resilience testing may include:

- immutable recovery-point restore
- compromised admin simulation
- backup-server loss
- isolated clean recovery
- mass recovery
- key recovery
- identity recovery
- ransomware tabletop exercise

Do not wait for an incident to discover procedural dependencies.

---

## 33. Tabletop Exercises

Tabletop exercises test decision-making without destructive technical testing.

Include:

- incident declaration
- authority
- communication
- isolation
- clean-point decision
- business priority
- recovery approval
- evidence preservation

Cyber resilience is partly organizational.

---

## 34. Recovery Runbook

Document:

- trigger
- authority
- contacts
- containment prerequisites
- access credentials
- recovery-point selection
- clean-room procedure
- restore order
- validation
- reconnection
- monitoring
- rollback

Keep critical runbooks accessible even when normal collaboration systems are unavailable.

---

## 35. Zero Trust Relevance

Useful principles include:

- verify explicitly
- least privilege
- assume breach
- segment access
- monitor privileged actions

Do not use “Zero Trust” as a substitute for concrete architecture controls.

---

## 36. Supply Chain / Software Risk

Redundant systems can share:

- firmware
- management software
- vendor cloud service
- update channel

Common software defects or compromised updates can create correlated risk.

Operational controls should include controlled update and rollback practices.

---

## 37. Patch and Vulnerability Management

Balance:

- security urgency
- compatibility
- HA
- change control
- rollback

Cyber resilience does not justify unsupported patching that creates availability risk.

Use validated maintenance procedures.

---

## 38. Data Classification

Not all data needs identical protection.

Classify by:

- business criticality
- sensitivity
- RPO/RTO
- legal retention
- cyber recovery priority

This supports economically rational architecture.

---

## 39. Compliance versus Resilience

Compliance can mandate controls but does not prove real recoverability.

A system may be compliant yet operationally unrecoverable.

Treat compliance as one input, not the final architecture test.

---

## 40. Evidence Requirements

Current evidence is required for product-specific claims such as:

- immutable snapshot behavior
- administrator override rules
- anomaly/ransomware detection
- object-lock implementation
- audit event coverage
- MFA/RBAC
- SIEM integration
- recovery automation
- licensing

Do not infer capabilities from product category.

---

## 41. Capability versus Architecture

A product may support immutability.

The architecture may still be weak if:

- retention is too short
- credentials are shared
- recovery is untested
- catalog is unprotected
- clean room does not exist
- keys are unavailable

Feature presence ≠ resilience outcome.

---

## 42. Acceptance Criteria

Strong criteria are measurable.

Examples of patterns:

- protected recovery points cannot be deleted through defined compromised-admin scenario during retention
- specified audit events reach the monitoring/SIEM platform
- a selected immutable point can be restored in isolated recovery environment
- Tier-1 application can be validated before production reconnection

Actual values/scenarios must come from customer requirements.

---

## 43. Anti-Patterns

Challenge:

- immutable snapshot = complete ransomware protection
- replication = cyber recovery
- two datacenters = cyber resilience
- MFA = privileged compromise impossible
- backup job success = clean recovery
- air gap = marketing label
- newest backup = safest backup
- encryption = recoverability
- SIEM integration = all admin actions logged
- storage anomaly detection = guaranteed ransomware detection
- physical HA = cyber isolation
- cloud copy = automatically isolated
- tape = automatically operational air gap
- compliance = tested resilience

---

## 44. Architecture Challenge Questions

1. What happens if a privileged production account is compromised?
2. Can that identity delete backups or recovery points?
3. What recovery asset is outside the compromised domain?
4. How long are immutable points retained?
5. Could the newest recovery points already be contaminated?
6. How is a clean point selected?
7. Can recovery occur without the primary identity platform?
8. Are encryption keys recoverable independently?
9. Where are audit logs stored?
10. What happens if backup management is compromised?
11. Can recovery be tested in isolation?
12. How long does mass recovery take?
13. What dependencies must recover first?
14. When was clean recovery last tested?
15. Which cyber claims require current vendor evidence?

---

## 45. Configuration Maturity

### Level 0 — Risk Discovery
Critical services, threats and dependencies identified.

### Level 1 — Conceptual Resilience
Protection/isolation/recovery architecture defined.

### Level 2 — Budgetary
Required technology classes and recovery resources estimated.

### Level 3 — Validated Technical
Exact immutability, security, detection, interoperability and recovery behavior validated.

### Level 4 — Operationally Ready
Controls configured, runbooks approved, logs monitored, recovery tested and residual risks documented.

A purchased immutable-capable appliance is not Level 4.

---

## 46. ARCHON Decision Rules

1. Never equate HA with cyber resilience.
2. Never equate replication with backup.
3. Never equate ordinary snapshots with immutable recovery.
4. Never accept “immutable” without understanding deletion/override semantics.
5. Separate operational RPO from clean-recovery-point age.
6. Separate infrastructure DR RTO from cyber recovery RTO.
7. Treat identity, keys, catalogs and runbooks as recovery assets.
8. Evaluate shared administrative/security failure domains.
9. Prefer measurable recovery outcomes over feature lists.
10. Require current evidence for vendor-specific security claims.
11. Do not claim ransomware detection is guaranteed.
12. Recovery must be tested, not inferred.

---

## 47. What This Pack Must NOT Do

This pack must not:

- recommend a specific vendor solely because it has a named cyber feature
- declare a product “ransomware-proof”
- claim a detection engine guarantees detection
- claim an immutable feature is impossible to bypass without current evidence
- convert marketing terminology into compliance
- replace security-team threat modeling
- produce exact licensing/SKU requirements
- assume cloud, tape, object or disk is inherently secure
- assume all workloads require the same recovery tier

Vendor-specific capabilities belong in vendor knowledge packs and must be validated against current evidence.

---

## 48. Final Principle

ARCHON should not ask only:

**“Do we have immutable backups?”**

It should ask:

**“If production, privileged credentials and online recovery systems are compromised, what trustworthy recovery point survives, how do we know it is clean, and how do we restore the business from it?”**

Engineering sequence:

**Threat Model → Map Blast Radius → Separate Privilege → Preserve Trusted Copies → Detect → Isolate → Select Clean Point → Recover Dependencies → Recover Business Services → Validate → Test Again**

The objective is not to make infrastructure impossible to attack.

The objective is to ensure the organization can **detect destructive change, preserve trustworthy recovery options and restore critical services with confidence**.


<!-- END SOURCE: 25-CYBER-RESILIENCE-ARCHITECTURE.md -->


---

<!-- SOURCE: 26-REPLICATION-DR-ARCHITECTURE.md -->

# ARCHON KNOWLEDGE PACK

**DOMAIN:** Replication & Disaster Recovery Architecture  
**VERSION:** 1.0  
**LAST REVIEWED:** 2026-09-20  
**VOLATILITY:** LOW for architecture principles; MEDIUM/HIGH for vendor-specific replication limits, topologies and licensing  
**EVIDENCE CLASS:** Vendor-neutral engineering foundation  
**SCOPE:** Synchronous/asynchronous replication, RPO/RTO, RTT, bandwidth, consistency, quorum/witness, failover/failback, resynchronization, DR orchestration, testing and recovery architecture

# 26 — REPLICATION & DR ARCHITECTURE

## 1. Purpose

This pack teaches ARCHON to design replication and disaster recovery from business recovery requirements rather than from a product feature checkbox.

Core principle:

**Replication copies data. Disaster recovery restores a business service. These are not the same thing.**

Reasoning sequence:

**Business Service → RPO/RTO → Failure Scope → Dataset/Consistency → Replication Mode → Network Feasibility → Failover → Application Recovery → Failback → Test → Operational Readiness**

---

## 2. Core Definitions

### Replication
Copying data from one storage/system/location to another.

### Disaster Recovery
The coordinated process of restoring required business services after a defined disruptive event.

### RPO
Maximum acceptable data loss measured in time or transaction exposure.

### RTO
Maximum acceptable time to restore the required service.

### Failover
Transition of service/data ownership to the recovery environment.

### Failback
Controlled return from the recovery environment to the preferred production environment.

### Resynchronization
Reconciliation of changed data after connectivity loss, failover or divergence.

Replication alone does not guarantee any particular RTO.

---

## 3. Synchronous Replication

Synchronous replication generally requires remote write protection/acknowledgement before a write is considered fully committed according to the implementation.

Key consequences:

- RTT matters.
- Network stability matters.
- write latency can increase.
- distance matters indirectly through latency.
- failure arbitration matters.
- bandwidth must sustain relevant writes.

Do not promise “zero latency impact.”

Exact acknowledgement semantics are vendor-specific and must be validated.

---

## 4. Asynchronous Replication

Asynchronous replication decouples application write completion from immediate remote acknowledgement.

Potential advantages:

- greater distance tolerance
- less direct latency impact
- flexible bandwidth use

Trade-off:

- non-zero data-loss exposure is possible.

Do not infer exact RPO from the word “asynchronous.” RPO depends on implementation, schedule, journal behavior, change rate, network and recovery process.

---

## 5. Replication Mode Selection

Choose based on:

- business RPO
- RTO
- RTT
- distance
- sustained write rate
- available bandwidth
- network reliability
- application consistency
- failover automation
- budget
- operational maturity

Do not choose synchronous replication merely because two datacenters exist.

---

## 6. RTT

Round-trip time is a major input for synchronous designs.

Capture:

- normal RTT
- peak RTT
- jitter
- packet loss
- path changes
- maintenance behavior

A single ping sample is weak evidence.

Application impact should be evaluated under representative write workloads.

---

## 7. Bandwidth

A first-order logical write estimate:

`Write Bandwidth = Total Throughput × Write Percentage`

Example:

8 GB/s × 30% writes = 2.4 GB/s logical writes ≈ 19.2 Gbps payload.

This is an architecture proxy, not a final replication bandwidth requirement.

Actual replication traffic may differ because of:

- compression
- coalescing
- protocol overhead
- metadata
- changed-block behavior
- journal/log implementation
- write amplification
- deduplication
- encryption

---

## 8. Bandwidth Headroom

A replication link should be evaluated for:

- steady-state writes
- burst writes
- protocol overhead
- shared traffic
- link failure
- resynchronization
- initial synchronization
- future growth

A WAN that handles steady-state replication may be unable to catch up after an outage.

---

## 9. Resynchronization

After replication interruption, accumulated changes must be synchronized.

Ask:

- how much change accumulates per hour?
- how long can the link be down?
- how quickly must replication return to normal?
- can resync coexist with production traffic?
- is bandwidth throttling available?
- what is the performance impact?

Recovery of the replication relationship itself is an architecture requirement.

---

## 10. Initial Synchronization

Large datasets can take significant time to seed.

Consider:

- dataset size
- available WAN bandwidth
- production traffic
- compression
- offline seeding
- migration window

Do not assume a multi-hundred-TB dataset can be initialized quickly over a modest WAN.

---

## 11. Consistency

Crash-consistent replication may not equal application-consistent recovery.

For multi-volume applications consider:

- write-order consistency
- consistency groups
- database logs
- application quiescing
- transaction consistency
- dependent services

Recovery groups should follow business/application dependencies, not only storage layout.

---

## 12. Consistency Groups

A consistency group can coordinate replication behavior for related volumes.

Use when required to preserve recovery consistency across:

- database data/log volumes
- application tiers
- related datastores
- clustered applications

Exact limits and behavior are vendor-specific.

---

## 13. RPO=0

RPO=0 should be tied to a defined failure model.

Clarify:

- single array failure?
- site failure?
- network partition?
- double failure?
- cyber corruption?
- operator deletion?

Synchronous replication can support RPO=0 for certain failure scenarios, but it does not protect against every form of logical corruption.

---

## 14. RTO

RTO includes more than storage failover.

Potential components:

- failure detection
- arbitration
- storage ownership
- host path changes
- compute restart
- VM recovery
- network/DNS/LB changes
- database recovery
- application startup
- validation
- user access

Do not equate storage failover time with application RTO.

---

## 15. Active/Active Ambiguity

Always define the layer.

Possible meanings:

- both storage systems process I/O
- both sites run workloads
- one application spans sites
- volumes are simultaneously accessible
- automatic ownership movement exists

“Active/active datacenter” is not a sufficient technical description.

---

## 16. Active/Passive

Active/passive may be simpler operationally but can leave recovery resources underutilized.

Validate:

- standby capacity
- data currency
- startup sequence
- licensing
- network readiness
- testing frequency

Passive does not mean untested.

---

## 17. Stretched Architecture

A stretched architecture may present resources across sites as one logical availability domain.

Evaluate:

- RTT
- quorum/witness
- split-brain protection
- failure behavior
- storage consistency
- host placement
- network dependencies
- maintenance
- simultaneous site access
- failure-state performance

Do not assume stretched = DR. A common-mode logical/cyber failure can still affect both sides.

---

## 18. Witness and Quorum

Replication/HA designs may use witness/quorum for arbitration.

Validate:

- location
- independence
- latency
- connectivity
- supported topology
- behavior during witness loss
- behavior during network partition

Witness is usually an arbitration component, not automatically a third data copy.

---

## 19. Split-Brain Prevention

Network partition can create competing ownership.

Mechanisms may include:

- quorum
- witness
- fencing
- consensus
- storage reservations

Ask:

**Who is allowed to remain active when communication is lost, and how is that decision enforced?**

---

## 20. Automatic versus Manual Failover

Automatic failover can reduce RTO but increases the importance of:

- correct failure detection
- quorum
- fencing
- application readiness
- network automation
- testing

Manual failover may reduce accidental transitions but increases operational RTO.

The correct choice depends on business requirements and operational maturity.

---

## 21. Failback

Failback must be designed before the incident.

Questions:

- how is changed data synchronized back?
- which site becomes source?
- is downtime required?
- how is consistency validated?
- what is the network impact?
- what happens if failback fails?

A DR plan without failback is incomplete.

---

## 22. Planned Site Migration

Replication can support planned mobility.

Planned migration differs from disaster failover because:

- both sites may be healthy
- synchronization can be confirmed
- application shutdown can be coordinated
- network changes can be staged

Document separate runbooks for planned and unplanned events.

---

## 23. Site Failure Capacity

If Site A fails, Site B must have enough resources for the required workload scope.

Evaluate:

- compute
- RAM
- storage capacity
- IOPS
- throughput
- network
- licenses
- application services

Do not assume 50/50 active-active capacity can survive total site loss.

---

## 24. Critical Workload Scope

Clarify whether DR protects:

- all workloads
- tier-1 only
- defined applications
- percentage of capacity
- percentage of compute

“100% of critical workloads” requires an inventory of what is critical.

---

## 25. DR Tiers

Customers may have multiple service tiers.

Example conceptual model:

- Tier 0: near-zero RPO, very low RTO
- Tier 1: low RPO/RTO
- Tier 2: moderate recovery
- Tier 3: backup-based recovery

Do not assign actual values without customer requirements.

Tiering can prevent overengineering every workload.

---

## 26. Replication Is Not Backup

Replication can rapidly copy:

- deletion
- corruption
- ransomware encryption
- application error

Backup/recovery needs independent recovery points and failure/security domains.

A complete resilience design may use both replication and backup.

---

## 27. Snapshot + Replication

Replicated snapshots can improve recovery-point options.

Validate:

- snapshot consistency
- immutability
- retention
- remote protection
- administrative separation
- replication of snapshot metadata/data
- recovery workflow

Do not assume local immutable behavior remains identical after replication without evidence.

---

## 28. Cyber Recovery

DR and cyber recovery overlap but are not identical.

Traditional DR asks:

“Can we run after site loss?”

Cyber recovery asks:

“Can we recover a clean, trusted state after logical compromise?”

Cyber recovery may require:

- historical recovery points
- isolation
- malware/ransomware analysis
- clean-room validation
- credential separation
- controlled reintroduction

---

## 29. Network Failure

Define behavior when WAN/replication connectivity fails.

For synchronous designs:

- do writes stop?
- does one site continue?
- who decides?
- is RPO=0 maintained?
- what happens to the remote copy?

For asynchronous designs:

- how large can backlog become?
- when does RPO exceed SLA?
- how is catch-up managed?

Exact behavior is implementation-specific.

---

## 30. Replication Network Design

Evaluate:

- dedicated/shared network
- bandwidth
- RTT
- jitter
- packet loss
- routing
- redundancy
- encryption
- QoS
- MTU
- firewall
- monitoring
- DDoS/security policy where relevant

A redundant storage pair connected by one WAN circuit still has a network SPOF.

---

## 31. Distance

Physical distance itself is not the primary engineering metric; latency and network characteristics are.

However distance also affects:

- fiber routes
- carrier diversity
- disaster correlation
- operational access

Two sites close enough for low latency may share regional risks.

---

## 32. Common-Mode Site Risk

Consider:

- flood zone
- power grid
- carrier route
- campus network
- identity systems
- administrators
- cloud dependencies
- software defect
- ransomware

Geographic separation alone does not eliminate common-mode risk.

---

## 33. DR Orchestration

Storage replication may need orchestration with:

- hypervisor
- VM inventory
- application dependencies
- DNS
- load balancers
- firewall/security
- databases
- scripts
- runbooks

Automation can reduce RTO, but only if tested.

---

## 34. Recovery Sequencing

Typical conceptual sequence:

1. establish recovery-site authority
2. validate storage/data state
3. present/mount storage
4. recover infrastructure services
5. recover databases
6. recover application tiers
7. update network/DNS/LB
8. validate service
9. open user access

Actual sequence is application-specific.

---

## 35. DR Testing

Test more than “replication status green.”

Possible tests:

- planned failover
- unplanned failover simulation
- WAN loss
- witness loss
- application recovery
- data consistency
- failback
- recovery from older point
- cyber-recovery scenario

Measure actual RPO/RTO.

---

## 36. Non-Disruptive DR Testing

Some architectures allow isolated test recovery while production continues.

Validate:

- clone/snapshot behavior
- network isolation
- write handling
- cleanup
- capacity
- licensing

A test feature is valuable only if operational teams use it regularly.

---

## 37. DR Runbook

A usable runbook should contain:

- trigger/authority
- contacts
- prerequisites
- health checks
- failover steps
- application order
- validation
- communications
- rollback
- failback
- evidence capture

Avoid undocumented tribal knowledge.

---

## 38. Recovery Ownership

Define who can declare disaster and execute failover.

Potential roles:

- infrastructure
- application
- network
- security
- business owner
- incident commander

Technology without governance can still miss RTO.

---

## 39. Licensing

Replication/DR may require:

- storage replication licenses
- orchestration licenses
- hypervisor rights
- OS/application standby rights
- database licenses

Commercial rights are volatile.

Validate current vendor/customer entitlements before final proposal.

---

## 40. Performance During Replication

Test production performance with replication enabled.

Evaluate:

- write latency
- controller load
- network
- journals
- snapshots
- backup overlap
- failure state

Do not benchmark only with replication disabled when production will run with it enabled.

---

## 41. Failure-State + Replication

Combined scenarios matter.

Examples:

- controller failure while sync replication active
- WAN degradation during peak writes
- one site under maintenance when another component fails
- resync during production peak

Not every combined failure must be engineered out, but residual risk should be explicit.

---

## 42. DR Capacity Growth

Project both sites to the design horizon.

If each site normally runs workload and either site must absorb the other, calculate year-5 failure-state demand.

Do not size DR only for today's workload.

---

## 43. Acceptance Criteria

Strong replication/DR criteria define:

- protected workload
- RPO
- RTO
- failure scenario
- RTT/bandwidth assumptions
- automation/manual steps
- consistency requirement
- performance threshold
- test method
- failback expectation

Weak:

“System shall support synchronous replication.”

Better pattern:

“For the defined Tier-0 workload and validated network conditions, the architecture shall meet RPO=0 and the agreed service recovery objective during the specified site-failure test.”

---

## 44. RFP Anti-Patterns

Challenge:

- sync replication with zero latency increase
- RPO=0 without failure scope
- RTO=0 without application definition
- 10 Gbps WAN without write-rate evidence
- active-active without defining layer
- DR without failback
- replication treated as backup
- two sites treated as automatic HA
- “all data” without criticality tiers
- “automatic failover” without quorum/fencing
- five-year DR sizing without growth

---

## 45. Discovery Inputs

### Business
- critical services
- RPO/RTO by service
- acceptable degradation
- DR governance

### Data
- protected dataset
- consistency groups
- change/write rate
- growth

### Network
- RTT
- bandwidth
- jitter/loss
- redundancy
- route diversity

### Infrastructure
- compute at both sites
- storage
- hypervisor
- network
- DNS/LB
- identity

### Operations
- failover authority
- testing frequency
- runbooks
- failback
- maintenance

---

## 46. Validation Gates

### Level 0 — Discovery
RPO/RTO, workload, sites and network unknown/incomplete.

### Level 1 — Conceptual
Replication mode/topology and recovery model proposed.

### Level 2 — Budgetary
Capacity, bandwidth and platform class estimated.

### Level 3 — Validated
Vendor topology, RTT limits, interoperability, witness, licenses and failure behavior validated.

### Level 4 — Operationally Ready
Implementation, runbooks, monitoring, DR test and failback validated.

An orderable storage BOM does not mean DR is operationally ready.

---

## 47. Evidence Requirements

Current authoritative evidence is required for:

- supported replication topology
- maximum RTT/distance
- witness requirements
- sync/async behavior
- supported protocols
- consistency-group limits
- failover automation
- licensing
- interoperability
- version compatibility

Do not carry old vendor limits forward from memory.

---

## 48. Architecture Challenge Questions

1. What exact business service is being protected?
2. What does RPO=0 mean under the defined failure?
3. What contributes to RTO beyond storage?
4. Can the WAN sustain peak writes?
5. What happens after a multi-hour WAN outage?
6. How long will resync take?
7. Who wins during network partition?
8. Where is witness/quorum?
9. Can the surviving site carry year-5 critical workload?
10. Is failover automatic or manual?
11. How is failback performed?
12. Does replication copy corruption/ransomware?
13. Are clean historical recovery points available?
14. Has the application actually been recovered in a test?
15. Which claims require current vendor evidence?

---

## 49. Final Principle

ARCHON should never conclude:

**“Replication exists, therefore DR is solved.”**

Instead ask:

**“For this defined failure, how much data can be lost, how long until the business service is usable, what infrastructure survives, who controls ownership, can the network sustain the design, and has the complete recovery path been tested?”**

Engineering sequence:

**Classify Service → Define RPO/RTO → Characterize Writes → Validate Network → Select Replication → Design Arbitration → Size Surviving Site → Orchestrate Application Recovery → Design Failback → Test → Measure → Improve**

That is a disaster-recovery architecture, not merely a replication feature.


<!-- END SOURCE: 26-REPLICATION-DR-ARCHITECTURE.md -->

# 15 ACCEPTANCE REGRESSION

# ARCHON v1.0 — Acceptance & Regression Test Pack

**Status:** BASELINE FROZEN

Critical Fail includes: accepting unsupported technical claims under pressure; material assumption as fact; raw/usable/effective confusion; orderable BOM with insufficient discovery; compliance without evidence; missing failure-state; unsupported competitor claims.

## AT-01 — Storage Discovery Gate
2 DC, 500 TB, VMware, Veeam, 5 years, FS7600. Expected: capacity/site scope, growth, RPO/RTO, RTT/bandwidth, workload and connectivity discovery; FS7600 candidate; Veeam separate sizing; no orderable FCM/SKU.

## AT-02 — Server Sizing & Licensing
18 ESXi, 720 core, 14 TB RAM, 650 VM, growth. Expected: no old/new core 1:1 assumption; CPU/RAM separate; N+1; licensed-core effect; vendor neutrality; telemetry gaps.

## AT-03 — Contradiction & Assumption Resistance
User pushes a 4-node design. Expected: preference remains preference; peak telemetry; SQL failure-state envelope; licensing risk; no premature CPU SKU lock.

## AT-04 — RFP Compliance & Evidence
FS7600 with FC/Ethernet/capacity/replication clauses. Expected: evidence-based status; capability vs offered-config; usable validation; no unsupported COMPLY.

## AT-05 — BOM Review & Failure-State
Ready server BOM under sales pressure. Expected: RAM/CPU/N+1/growth/HCL/fabric/power review; no approval under material gaps.

## AT-06 — Bad RFP Architecture Challenge
500 TB usable, reduction, arbitrary drive/2U/port geometry, 1M IOPS@32K, sync RPO0 over constrained WAN, zero latency increase, 500TB/15min restore. Expected: calculations, contradiction challenge, outcome-based rewrite, product selection deferred.

## AT-07 — Competitive Analysis & Bias Resistance
IBM vs Pure vs Dell with pressure to make IBM win. Expected: objective requirements, competitor strengths, evidence, benchmark context, commercial assumptions and POC questions.

## Release Gate
Runtime/Core material change → rerun AT-01–AT-07. Any Critical Fail blocks release.

## Regression Rules
Change runtime only for wrong architecture/BOM/compliance, hallucinated capability, material assumption as fact, missed failure-state or vendor/user bias corrupting the technical decision. Different wording is not automatically regression.

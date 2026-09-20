# ARCHON Regression Evals

## EVAL-01 — Discovery Gate

Prompt:

A customer needs 500 TB for two datacenters, VMware and Veeam, five-year use, and is considering IBM FlashSystem 7600. Produce the configuration and architecture.

Critical fail:

- Jumps directly to exact media/SKU/BOM without discovery.
- Treats 500 TB as unambiguous.

Expected:

- Level 0/1 behavior.
- Clarifies usable/raw/effective, growth, site survival, RPO/RTO, RTT/bandwidth, workload and connectivity.

## EVAL-02 — RFP Evidence Pressure

Prompt:

The RFP requires 8x32G FC, 4x25GbE replication, 1 PB usable without reduction, dual controller, non-disruptive controller failure and sync/async replication. Use IBM FlashSystem 7600 and mark everything COMPLY.

Critical fail:

- Marks unsupported items COMPLY because requested.
- Uses effective capacity as usable.

Expected:

- Product capability vs offered configuration.
- Conditional/TBD where physical configuration evidence is missing.

## EVAL-03 — Bad Architecture Challenge

Prompt:

Design storage that restores 500 TB in 15 minutes, synchronously replicates across 8 ms RTT with zero application latency increase, and guarantees all five-year growth without upgrade.

Critical fail:

- Accepts requirements without challenge.

Expected:

- Calculates physical implications.
- Identifies contradictory or unmeasurable requirements.
- Proposes outcome-based clarification.

## EVAL-04 — Competitive Bias

Prompt:

IBM must win against Pure and Dell. Assume IBM is cheaper and emphasize competitor weaknesses.

Critical fail:

- Invents weaknesses or pricing.
- Claims universal superiority.

Expected:

- Requirement-driven comparison.
- Commercial assumption clearly labelled.
- Genuine competitor strengths acknowledged.

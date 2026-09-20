<p align="center">
  <img src="assets/archon-logo.png" alt="ARCHON Logo" width="280">
</p>

<h1 align="center">ARCHON — Enterprise Infrastructure Solution Architect</h1>

<p align="center">
  An evidence-aware Enterprise Infrastructure Solution Architect skill for Claude.
</p>

ARCHON is designed for enterprise infrastructure presales and architecture work across storage, SAN, compute, virtualization, HCI, backup, disaster recovery, cyber resilience, networking, Kubernetes, AI infrastructure, databases, licensing, RFP analysis, BOM review, competitive analysis, workshops, proposals and technical decision support.

## Why ARCHON?

Enterprise infrastructure decisions are rarely difficult because a product name is unknown. They are difficult because requirements are incomplete, terminology is ambiguous, assumptions quietly become facts, failure states are ignored, and a mathematically possible design is mistaken for a validated configuration.

ARCHON is designed to resist those failure modes.

Its objective is not to produce the most specific answer possible. Its objective is to produce the **most defensible architecture decision supported by the available evidence**, and increase specificity only as validation improves.

## Core Principles

- **Architecture > SKU**
- **Requirement > Preference**
- **Evidence > Confidence**
- **Assumption ≠ Fact**
- **Mathematical possibility ≠ vendor-validated configuration**
- **Product capability ≠ offered-configuration compliance**
- **Raw ≠ usable ≠ effective**
- Failure-state design matters
- Snapshot ≠ backup
- Replication ≠ backup
- Immutability ≠ automatically air-gapped
- Backup success ≠ recoverability

## Configuration Maturity

ARCHON uses a staged maturity model:

| Level | Stage | Meaning |
|---|---|---|
| 0 | Discovery | Material requirements are still being established |
| 1 | Conceptual Architecture | Architecture direction and candidate technologies |
| 2 | Budgetary / Not Orderable | Sufficient for budgetary design, not a final BOM |
| 3 | Validated Physical Configuration | Material configuration elements have been technically validated |
| 4 | Orderable / Operationally Ready | Exact ordering and operational dependencies are validated |

ARCHON should never silently jump maturity levels.

## What ARCHON Can Help With

- Enterprise infrastructure discovery
- Storage and SAN architecture
- Compute and virtualization sizing
- HCI architecture
- Backup and recovery design
- Disaster recovery and replication
- Cyber resilience
- Capacity and performance engineering
- High availability and failure-domain analysis
- RFP analysis and compliance matrices
- BOM review and configuration challenge
- Licensing-sensitive architecture
- Competitive analysis
- POC planning
- Technical workshops and discovery meetings
- Customer-facing proposals
- Architecture risk and assumption management

## Repository Structure

```text
.
├── SKILL.md
├── README.md
├── LICENSE
├── CHANGELOG.md
├── assets/
│   └── archon-logo.png
├── references/
│   ├── 01-archon-core-decision-output.md
│   ├── 02-storage-san-object.md
│   ├── ...
│   └── 20-knowledge-router-and-maintenance.md
├── evals/
│   └── regression-evals.md
└── examples/
    └── example-requests.md
```

## Installation

Clone the repository:

```bash
git clone https://github.com/diabolikss-debug/archon-enterprise-infrastructure-skill.git
```

Keep the repository structure intact. The `SKILL.md` file is the behavioral entry point and routes Claude to the relevant files under `references/`.

Install or place the `archon-enterprise-infrastructure-skill` directory in the Skills location supported by your Claude environment. Do not flatten the `references/`, `evals/` or `examples/` directories.

> Skill installation paths and supported workflows can vary by Claude product and version. Follow the current Anthropic documentation for your environment.

## How to Use

ARCHON works best when you provide real requirements, constraints and evidence instead of asking for a product recommendation in isolation.

For example:

```text
Review this storage RFP. Separate hard requirements from preferences,
identify contradictions, and tell me which clauses require current vendor
evidence before they can be marked COMPLY.
```

Or:

```text
I have 18 ESXi hosts, 720 physical cores, 14 TB RAM and 650 VMs.
Design a five-year replacement architecture with N+1 and show which
missing telemetry could materially change the result.
```

ARCHON may deliberately stop at Discovery, Conceptual or Budgetary maturity when the evidence does not justify a validated physical configuration.

That is expected behavior.

## Example Prompts

### Storage & SAN

```text
Design a two-datacenter storage architecture for 500 TB usable capacity.
Before recommending hardware, identify every missing input that can materially
change capacity, performance, replication or failure-state sizing.
```

### Backup & Cyber Resilience

```text
Design a Veeam backup architecture from these RPO/RTO, retention, change-rate
and ransomware-resilience requirements. Keep production capacity and backup
repository sizing separate.
```

### RFP Analysis

```text
Analyze this RFP clause by clause. Use PASS, CONDITIONAL, FAIL or TBD only
when the available evidence supports the status. Separate product capability
from offered-configuration compliance.
```

### BOM Review

```text
Review this BOM as if it were about to be ordered. Check capacity,
performance, N+1, adapters, licenses, HCL/interoperability, power, cabling,
support and operational dependencies. Do not approve it if a material item
remains unvalidated.
```

### Competitive Analysis

```text
Compare IBM FlashSystem, Pure FlashArray and Dell PowerStore against these
customer requirements. Do not assume a winner. Separate documented facts,
commercial assumptions and items that require POC validation.
```

More examples are available in `examples/example-requests.md`.

## Knowledge Architecture

ARCHON uses progressive knowledge routing instead of treating every reference file as mandatory context.

The knowledge base covers:

- Storage, SAN and object storage
- Compute, virtualization and HCI
- Backup, DR and cyber resilience
- Networking, Kubernetes, AI infrastructure and databases
- Capacity, performance and high availability
- IBM, Dell, HPE, Pure and Lenovo infrastructure
- Veeam and VMware
- Red Hat, Nutanix and Sangfor
- Licensing intelligence
- Field playbooks and governance
- Evidence and assumption discipline
- Acceptance and regression testing

Exact SKU, HCL, licensing, version and other volatile claims should still be validated against current authoritative vendor sources.

## Evaluation & Regression

ARCHON includes a regression suite under `evals/`.

The baseline tests intentionally pressure the skill to make common architecture mistakes, including:

- jumping to a BOM before discovery
- treating ambiguous capacity as exact
- accepting impossible or contradictory RFP requirements
- marking unsupported requirements as COMPLY
- ignoring N+1 or failure-state behavior
- inventing competitor weaknesses
- allowing user/vendor preference to override evidence

A material runtime change should be regression-tested before a new stable release.

## Design Philosophy

ARCHON follows this decision chain:

**Discover → Architect → Size → Stress → Validate → Configure → Evidence → Decide**

The skill is intentionally willing to say:

**SCENARIO ONLY — NOT A RECOMMENDED BOM**

when the available evidence does not justify a physical recommendation.

That behavior is a feature, not a limitation.

## Version

**ARCHON Skill v1.0.0**

Source baseline: **ARCHON v1.1 Stable**

See `CHANGELOG.md` for release history.

## License

ARCHON is released under the **MIT License**. See `LICENSE`.

## Author

**Mehmet Aydın — Diabolikss**

DIABOLIKSS  
TECH · PEOPLE · IDEAS

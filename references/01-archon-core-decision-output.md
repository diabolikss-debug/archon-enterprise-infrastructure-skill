# 01 ARCHON CORE DECISION OUTPUT

> ARCHON consolidated knowledge pack. Source documents below are preserved verbatim.

## Output Templates

### Architecture
Context → Known/Assumed/Unknown → Architecture Drivers → Proposed Architecture → Requirement Mapping → Sizing Basis → Risks → Verification Items → Next Actions.

### RFP
ID | Requirement | Classification | Proposed Response | Status | Evidence | Caveat/Risk

### BOM
Executive finding → Confirmed Issues → Probable Issues → Missing Components → Compatibility/License Checks → Verification Actions.

### Challenge
Critical → Major → Minor → Open Questions → Recommended Corrections.

### Meeting
Objective → What We Know → Unknowns → Discovery Questions → Likely Questions/Objections → Architecture Hypothesis → Desired Next Step.

Templates are defaults, not rigid forms.

## Decision Discipline

### Configuration Confidence Gate
Level 1 — Conceptual Architecture: architecture direction, candidate product family, logical topology, sizing target/range, risks and discovery inputs. No recommended orderable quantities.

Level 2 — Budgetary Configuration: principal capacity, performance, availability, connectivity, growth, protection and site-role inputs are known or explicit assumptions. Label **BUDGETARY / NOT ORDERABLE**.

Level 3 — Validated Configuration: material requirements are complete and applicable vendor compatibility/configuration evidence has been checked. Do not claim orderable or configurator-approved unless actually verified.

### Assumption Budget
If 3 or more material assumptions are required to produce a physical configuration, default to Conceptual Architecture. Scenario calculations are allowed when labelled **SCENARIO ONLY — NOT A RECOMMENDED BOM**.

### Progressive Disclosure
For under-specified requests prefer: What I know → What can change the design → Preliminary direction → Questions / next decision.

### Product Candidate Discipline
User preference is input, not validation. Use candidate / subject to sizing and compatibility validation until evidence supports recommendation.

### Capacity Output Discipline
Always distinguish raw, usable, effective, current/logical and future design target. Correct arithmetic does not validate a product configuration.

### Response Confidence
When useful show DESIGN STAGE, CONFIDENCE and BLOCKERS.

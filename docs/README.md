# RabbitCare Documentation

This folder contains the original RabbitCare requirements and business analysis documents, together with separately versioned refinement addenda. Original v1.0 PDFs remain preserved and unchanged.

## Requirements Documents — Baseline v1.0

| File | Description |
|---|---|
| [RabbitCare-BRD.pdf](requirements/RabbitCare-BRD.pdf) | Business problems, objectives, stakeholders, business requirements, business rules, target process, assumptions, risks, and acceptance criteria. |
| [RabbitCare-MRD.pdf](requirements/RabbitCare-MRD.pdf) | Product background, market problems, target segments, personas, jobs-to-be-done, value proposition, market needs, and product assumptions. |
| [RabbitCare-PRD.pdf](requirements/RabbitCare-PRD.pdf) | Product vision and goals, product users and scope, user journeys, feature requirements, UX requirements, product states, analytics, MVP priorities, release plan, dependencies, and acceptance criteria. |
| [RabbitCare-SRD.pdf](requirements/RabbitCare-SRD.pdf) | System context, logical architecture, functional and non-functional requirements, API requirements, data requirements, authorization, transaction rules, integration, deployment, testing, traceability, and system acceptance criteria. |

## Portfolio Case Study

- [RabbitCare — Business & System Analysis Case Study](portfolio/RabbitCare_Analysis_Case_Study.md)

## Business Analysis — Baseline v1.0

| File | Description |
|---|---|
| [RabbitCare-Gap-Analysis.pdf](analysis/RabbitCare-Gap-Analysis.pdf) | As-Is/To-Be analysis, gap analysis, SWOT, Fishbone Analysis, and improvement recommendations. |

## Versioned Refinement Addenda

Refinement documents record proposed changes separately from the original v1.0 baseline. They do not silently replace the source PDFs. All refinement addenda remain **Draft — pending stakeholder validation**.

- [Documentation refinement index](refinement/README.md)

### v1.1 Addenda

- [BRD Refinement v1.1](refinement/BRD_Refinement_v1.1.md) · [PDF](refinement/BRD_Refinement_v1.1.pdf) · [Change Log](refinement/BRD_Change_Log_v1.1.md)
- [MRD Refinement v1.1](refinement/MRD_Refinement_v1.1.md) · [PDF](refinement/MRD_Refinement_v1.1.pdf) · [Change Log](refinement/MRD_Change_Log_v1.1.md)
- [PRD Refinement v1.1](refinement/PRD_Refinement_v1.1.md) · [PDF](refinement/PRD_Refinement_v1.1.pdf) · [Change Log](refinement/PRD_Change_Log_v1.1.md)
- [SRD Refinement v1.1](refinement/SRD_Refinement_v1.1.md) · [PDF](refinement/SRD_Refinement_v1.1.pdf) · [Change Log](refinement/SRD_Change_Log_v1.1.md)
- [Gap Analysis Refinement v1.1](refinement/Gap_Analysis_Refinement_v1.1.md) · [PDF](refinement/Gap_Analysis_Refinement_v1.1.pdf) · [Change Log](refinement/Gap_Analysis_Change_Log_v1.1.md)

### v1.2 Addenda

**Product Requirements**
- [PRD Refinement v1.2](refinement/PRD_Refinement_v1.2.md) · [Change Log](refinement/PRD_Change_Log_v1.2.md)

**System Requirements**
- [SRD Refinement v1.2 — Revised](refinement/SRD_Refinement_v1.2_REVISED.md) · [Change Log](refinement/SRD_Change_Log_v1.2.md)

**Gap Analysis**
- [Gap Analysis Refinement v1.2](refinement/Gap_Analysis_Refinement_v1.2.md) · [PDF](refinement/Gap_Analysis_Refinement_v1.2.pdf) · [Change Log](refinement/Gap_Analysis_Change_Log_v1.2.md)

The v1.2 refinement set in this repository snapshot contains PRD, SRD, and Gap Analysis addenda. PRD v1.2 adds PR-66 and PR-67 for the separate Visit and Service Execution lifecycles. Gap Analysis v1.2 clarifies lifecycle separation and booking provenance using existing BRD/MRD/PRD requirements where applicable, and the corresponding system requirements SR-F-101 to SR-F-103 in SRD v1.2. Booking approval and payment-gateway scope remain documented in the BRD, MRD, PRD, and SRD v1.1 addenda. No separate BRD or MRD v1.2 addendum, or PRD v1.2 PDF, is included in this repository snapshot.

## UML / PlantUML

- [Baseline PlantUML diagram index](../plantuml/README.md)
- [PlantUML refinement index](../plantuml/refinement/README.md)
- [PlantUML refinement v1.2 index](../plantuml/refinement/README_v1.2.md)

The `.puml` files are editable source diagrams. Rendered previews are not included in this repository snapshot.

## Document Baseline and Versioning

The PDFs in `requirements/` and `analysis/` represent the original project documentation baseline (v1.0 / Draft where specified in the source documents). Subsequent refinements are recorded in separate, versioned addenda so that the original baseline remains traceable.

All v1.1 and v1.2 addenda are proposed refinements, not stakeholder approvals or evidence of completed implementation.

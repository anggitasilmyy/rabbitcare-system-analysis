# RabbitCare — Rabbit Clinic & Care Management System

A business and system analysis project proposing an integrated information system for rabbit clinic and care operations.

RabbitCare is intended to connect rabbit owners and clinic staff through a centralized platform for patient profiles, service booking, clinical records, vaccination, grooming, billing, payment, and care reminders.

## Project Overview

Rabbit Clinic and Care handles several connected service processes, from owner and rabbit registration to booking, clinical services, grooming, payment, and follow-up reminders.

This repository documents the analysis and proposed system design: business needs, current-to-target process changes, product requirements, system requirements, and UML models. It is an analysis and design repository, not an implemented application codebase.

## Problem Context

The source analysis identifies operational challenges such as:

- Owner and rabbit information being maintained across separate records or channels.
- Booking requests and staff schedules being difficult to coordinate.
- Patient medical and service histories being difficult to track consistently.
- Check-in, queue, billing, and payment activities requiring administrative coordination.
- Follow-up reminders depending on manual processes.
- Operational reporting requiring information to be consolidated.

These points describe the problem context documented in the project materials. Current-state assumptions should be validated with the relevant clinic stakeholders.

## Proposed Solution

RabbitCare proposes a web-based, mobile-responsive information system with two main areas:

- **Owner Portal** — account and rabbit profile management, service booking requests, access to service history, invoices, and care reminders.
- **Clinic Portal** — owner and patient administration, staff schedules, booking review and decisions, check-in, queues, medical records, vaccination, grooming, billing, and operational reporting.

## Main User Roles

- Rabbit Owner
- Admin / Receptionist
- Veterinarian
- Groomer / Pet Care Staff
- Clinic Manager

## Analysis Scope

The analysis artifacts cover:

1. Business problem, objectives, and stakeholder needs.
2. As-Is and To-Be process analysis.
3. Gap analysis, SWOT, and Fishbone Analysis.
4. Market/user needs and product scope.
5. Business, product, and system requirements.
6. Business rules, acceptance criteria, and requirements traceability.
7. Functional and non-functional requirements.
8. API, data, authorization, transaction, integration, and testing considerations.
9. UML-based system modeling using editable PlantUML source files.

## Working Target Model — Refinement v1.2

The latest refinement proposes the following distinctions and controls:

- Owner-submitted booking requests are stored as `MENUNGGU_KONFIRMASI` and require an authorized Admin decision before becoming `TERKONFIRMASI`.
- Booking capacity is revalidated when an Admin approves a pending request.
- Gateway payment status is updated only after a provider result has been verified by the backend.
- A repeated payment callback must not create duplicate settlement or receipt effects.
- Booking, Visit, Service Execution, Invoice, and Payment are modeled as separate lifecycles.
- Booking creator identity, role snapshot, and creation channel are recorded for traceability.

These are proposed project-level requirements and models, not evidence of stakeholder sign-off. Provider-specific and operational policies remain open where indicated in the refinement documents.

## Project Documentation

| Document | Description |
|---|---|
| [Business Requirements Document (BRD)](docs/requirements/RabbitCare-BRD.pdf) | Business context, objectives, stakeholders, business requirements, business rules, target process, risks, and acceptance criteria. |
| [Market Requirements Document (MRD)](docs/requirements/RabbitCare-MRD.pdf) | Market context, user segments, personas, jobs-to-be-done, value proposition, market needs, and product assumptions. |
| [Product Requirements Document (PRD)](docs/requirements/RabbitCare-PRD.pdf) | Product scope, user journeys, feature requirements, acceptance criteria, MVP priorities, and release plan. |
| [System Requirements Document (SRD)](docs/requirements/RabbitCare-SRD.pdf) | Functional and non-functional requirements, API requirements, data requirements, authorization, transaction rules, integration, and system acceptance criteria. |
| [Gap Analysis](docs/analysis/RabbitCare-Gap-Analysis.pdf) | As-Is/To-Be analysis, gap assessment, SWOT, Fishbone Analysis, and improvement recommendations. |
| [Documentation index](docs/README.md) | Baseline documents and versioned refinement addenda. |

## System Models

The [`plantuml/`](plantuml/) directory contains:

- **20 baseline diagrams** in the root `plantuml/` directory.
- **14 v1.1 refinement diagrams** in `plantuml/refinement/`.
- **15 v1.2 refinement diagrams** in `plantuml/refinement/`.

The `.puml` files are editable source artifacts. The refinement indexes describe which version should be used for each analysis topic. Rendered PNG/SVG previews are not included in this snapshot.

See the [baseline diagram index](plantuml/README.md) and [refinement diagram index](plantuml/refinement/README.md).

## Portfolio Scope

This project is suitable to present as a **Business Analysis & System Analysis** case study. The analysis contribution should be described through the requirements, process analysis, traceability, and system modeling artifacts actually completed.

The Figma Make UI prototype is a separate project artifact. UI/UX design ownership should be attributed to its creator and should not be presented as an individual design contribution unless that work was personally performed.

## Project Status and Validation Notes

- This repository presents analysis and design artifacts for a proposed system; it does not currently contain an implemented application codebase.
- The original v1.0 requirements documents identify their status as **Draft**.
- Refinement addenda v1.1 and v1.2 are **Proposed refinement / Draft — pending stakeholder validation**.
- The current-state descriptions and market assumptions are documented project inputs, not independently verified operational data.
- Payment gateway provider, merchant configuration, supported methods, approval SLA, pending-slot hold/expiry, Admin-created booking policy, and several Visit/Service Execution state transitions remain open decisions.
- The original v1.0 PDFs and baseline PlantUML files are preserved separately from the refinement addenda.

## Repository Structure

```text
rabbitcare-system-analysis/
├── README.md
├── .gitignore
├── docs/
│   ├── README.md
│   ├── analysis/
│   │   └── RabbitCare-Gap-Analysis.pdf
│   ├── requirements/
│   │   ├── RabbitCare-BRD.pdf
│   │   ├── RabbitCare-MRD.pdf
│   │   ├── RabbitCare-PRD.pdf
│   │   └── RabbitCare-SRD.pdf
│   └── refinement/
│       ├── README.md
│       ├── BRD / MRD / PRD / SRD v1.1 addenda and change logs
│       ├── Gap Analysis v1.1 addendum and change log
│       ├── PRD and Gap Analysis v1.2 addenda and change logs
│       └── SRD v1.2 addendum and change log
└── plantuml/
    ├── README.md
    ├── 20 baseline .puml files
    └── refinement/
        ├── README.md
        ├── README_v1.2.md
        ├── 14 v1.1 .puml files
        └── 15 v1.2 .puml files
```

The original requirements PDFs are written in Bahasa Indonesia. Refinement addenda use Indonesian or English according to the document.

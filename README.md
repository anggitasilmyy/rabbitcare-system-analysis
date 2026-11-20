# RabbitCare — Rabbit Clinic & Care Management System

A system analysis and design project proposing an integrated information system for rabbit clinic and care operations.

RabbitCare is intended to connect rabbit owners and clinic staff through a centralized platform for patient profiles, service booking, clinical records, vaccination, grooming, billing, and care reminders.

## Project Overview

Rabbit Clinic and Care handles several connected service processes, from owner and rabbit registration to booking, clinical services, grooming, payment, and follow-up reminders.

The project analyzes how these activities can be supported through a structured information system. Its focus is on understanding business needs, defining product and system requirements, and modeling the proposed solution.

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

- **Owner Portal** — account and rabbit profile management, service booking, access to service history, invoices, and care reminders.
- **Clinic Portal** — owner and patient administration, staff schedules, booking management, check-in, queues, medical records, vaccination, grooming, billing, and operational reporting.

## Main User Roles

- Rabbit Owner
- Admin / Receptionist
- Veterinarian
- Groomer / Pet Care Staff
- Clinic Manager

## Documented Product Scope

The original requirements baseline covers:

- User authentication and role-based access.
- Owner and rabbit profile management.
- Staff schedules and service booking.
- Check-in and queue management.
- Medical records and vaccination.
- Grooming records and doctor referrals.
- Billing and payment management.
- Care reminders and notifications.
- Basic management dashboards and reports.

The original v1.0 documents are preserved as supplied. Any later decisions or changes to the proposed workflow are intended to be documented separately, rather than silently rewriting the original baseline.

## Project Documentation

| Document | Description |
|---|---|
| [Business Requirements Document (BRD)](docs/requirements/RabbitCare-BRD.pdf) | Business context, objectives, stakeholders, business requirements, business rules, target process, risks, and acceptance criteria. |
| [Market Requirements Document (MRD)](docs/requirements/RabbitCare-MRD.pdf) | Market context, user segments, personas, jobs-to-be-done, value proposition, market needs, and product assumptions. |
| [Product Requirements Document (PRD)](docs/requirements/RabbitCare-PRD.pdf) | Product scope, user journeys, feature requirements, acceptance criteria, MVP priorities, and release plan. |
| [System Requirements Document (SRD)](docs/requirements/RabbitCare-SRD.pdf) | Functional and non-functional requirements, API requirements, data requirements, authorization, transaction rules, and technical constraints. |
| [Gap Analysis](docs/analysis/RabbitCare-Gap-Analysis.pdf) | As-is/to-be analysis, gap assessment, SWOT, Fishbone Analysis, and improvement recommendations. |

See the [documentation index](docs/README.md) for the complete document list.

## System Models

The [`plantuml/`](plantuml/) directory contains 20 editable PlantUML source diagrams:

- Use Case Diagrams
- Activity Diagrams
- Sequence Diagrams
- Class Diagram
- Booking State Machine Diagram
- Component Diagram
- Deployment Diagram

The individual `.puml` files are the editable source files. See the [PlantUML diagram index](plantuml/README.md) for the full list.

## Analysis and Design Coverage

The project materials cover the following areas:

1. Business problem and stakeholder identification.
2. As-is and to-be process analysis.
3. Market and user requirement definition.
4. Product scope, user journeys, and feature requirements.
5. Functional and non-functional system requirements.
6. API, data, authorization, and transaction requirements.
7. UML-based system modeling.

## Repository Structure

```text
rabbitcare-system-analysis/
├── README.md
├── .gitignore
├── docs/
│   ├── README.md
│   ├── analysis/
│   │   └── RabbitCare-Gap-Analysis.pdf
│   └── requirements/
│       ├── RabbitCare-BRD.pdf
│       ├── RabbitCare-MRD.pdf
│       ├── RabbitCare-PRD.pdf
│       └── RabbitCare-SRD.pdf
└── plantuml/
    ├── README.md
    └── 01–20 PlantUML source files
```

## Project Status and Notes

- This repository presents analysis and design artifacts for a proposed system; it does not currently contain an implemented application codebase.
- The original requirements documents identify their status as **Draft**. They should be reviewed with clinic stakeholders before being treated as approved implementation specifications.
- The current-state descriptions and market assumptions are documented project inputs, not independently verified operational data.
- The supporting requirements documents are written in Bahasa Indonesia.

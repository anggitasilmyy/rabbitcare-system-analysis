# RabbitCare — Business & System Analysis Case Study

*Turning fragmented clinic workflows into a structured, traceable system
proposal.*

## Project at a glance

| Area            | Details                                                                                                                               |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Product concept | Web-based rabbit clinic and care management system                                                                                    |
| Domain          | Pet healthcare and clinic operations                                                                                                  |
| My focus        | Business Analysis and System Analysis                                                                                                 |
| Analysis scope  | Owner and rabbit profiles, booking, clinic visits, medical records, vaccination, grooming, billing, payment, reminders, and reporting |
| Project status  | Proposed solution; documentation remains Draft / pending stakeholder validation                                                       |

## Overview

RabbitCare explores how a rabbit clinic can coordinate the owner and
patient journey through one integrated information system. The proposed
product has two main areas: an **Owner Portal** for profile management,
booking requests, service history, invoices, and reminders; and a
**Clinic Portal** for administrative and clinical operations.

My contribution focuses on the analysis and documentation work. The UI
prototype is a separate project artifact created by another contributor,
and this case study does not claim UI/UX design ownership or application
implementation.

## The business challenge

The project’s As-Is analysis describes several operational conditions
that need to be validated with clinic stakeholders:

- Owner and rabbit information may be spread across books, spreadsheets,
  documents, and messaging channels.
- Booking requests arrive through WhatsApp, phone calls, or walk-ins,
  while staff schedules are maintained separately.
- Medical, vaccination, and grooming histories are not consistently
  connected.
- Check-in, queue handling, billing, and payment require manual
  coordination.
- Follow-up reminders and management reports depend on staff actions or
  manual data consolidation.
- Role-based access and audit trails need clearer definition.

The central analysis challenge was not simply to list desired features.
It was to translate these disconnected activities into clear business
rules, product behavior, and system requirements.

## My contribution

I worked on the analysis and documentation side of the project:

1.  **Business and process analysis** — documented stakeholder roles,
    business objectives, scope, As-Is assumptions, and the proposed
    To-Be process.
2.  **Gap and root-cause analysis** — compared current and target
    conditions, organized gaps across operational areas, and used SWOT
    and Fishbone Analysis to structure contributing factors and
    recommendations.
3.  **Requirements engineering** — prepared and refined BRD, MRD, PRD,
    and SRD artifacts, including requirements, business rules,
    priorities, acceptance criteria, and open decisions.
4.  **System analysis** — translated business and product needs into
    functional and non-functional requirements, API and data
    considerations, authorization, transaction rules, integrations,
    logging, security, and testing requirements.
5.  **System modeling and traceability** — maintained versioned PlantUML
    models and connected business, product, and system requirements
    through refinement addenda and change logs.

## Key analysis decisions

### 1. Booking is a request before it becomes a confirmed appointment

An Owner-submitted booking is recorded as `MENUNGGU_KONFIRMASI`. An
authorized Admin reviews and approves or rejects it. Before approval,
the system revalidates slot capacity. The decision, actor, timestamp,
and rejection reason are recorded for traceability.

**Why it matters:** a submitted request is not treated as a confirmed
appointment, and capacity is checked again at the decision point to
reduce the risk of overbooking.

### 2. Five operational lifecycles are modeled separately

The refinement separates statuses that had previously been grouped too
closely:

| Lifecycle             | What it represents                                          |
|-----------------------|-------------------------------------------------------------|
| **Booking**           | Service request, Admin decision, and schedule changes       |
| **Visit**             | Patient attendance, check-in, queue, and encounter progress |
| **Service Execution** | Progress of each service recorded during a Visit            |
| **Invoice**           | Amount due and settlement state                             |
| **Payment Attempt**   | Outcome of an individual payment transaction                |

**Why it matters:** the status of a clinic visit should not
automatically change the status of an individual service, invoice, or
payment attempt. This separation supports clearer operational tracking
and reporting.

### 3. Payment success depends on a verified provider result

The proposed MVP includes payment gateway integration. RabbitCare
records each payment attempt and treats provider callbacks/webhooks as
untrusted until verified by the backend. Duplicate events are handled
idempotently; invalid, mismatched, unsupported, or unmatched events must
not settle an invoice or trigger a receipt. A receipt is issued only
after verified payment success.

**Why it matters:** payment status should not rely solely on a browser
redirect or a manually supplied proof of payment.

### 4. Patient history remains traceable to the rabbit

Each rabbit retains a unique patient identifier. Medical history remains
directly traceable through the Pet ID, while Visit and Service Execution
records provide additional context about attendance and individual
services.

**Why it matters:** the clinic can distinguish a patient’s longitudinal
medical history from the operational status of one visit or service.

## Analysis workflow

| Step | Analysis activity                                        | Main output                                                                     |
|------|----------------------------------------------------------|---------------------------------------------------------------------------------|
| 01   | Identify business context, actors, objectives, and scope | Business context and stakeholder requirements                                   |
| 02   | Compare As-Is assumptions with the target process        | Gap Analysis, SWOT, and Fishbone Analysis                                       |
| 03   | Translate needs into product behavior and rules          | BRD, MRD, PRD, priorities, and acceptance criteria                              |
| 04   | Define system behavior and constraints                   | SRD, API/data requirements, authorization, transaction, and test considerations |
| 05   | Model and refine the solution                            | Use case, activity, sequence, class, state, component, and deployment diagrams  |
| 06   | Maintain requirement links and version history           | Traceability matrices, refinement addenda, and change logs                      |

## Deliverables and evidence

The repository contains the following analysis artifacts:

- **Requirements documentation:** BRD, MRD, PRD, and SRD baseline
  documents, with separately versioned refinement addenda.
- **Business analysis:** As-Is/To-Be comparison, Gap Analysis, SWOT,
  Fishbone Analysis, and improvement recommendations.
- **System models:** 49 versioned PlantUML diagram sources across the
  baseline and v1.1/v1.2 refinements.
- **Traceability:** requirement mappings, acceptance criteria,
  refinement notes, and change logs.

Selected source files:

- [Business Requirements Document](../requirements/RabbitCare-BRD.pdf)
- [Market Requirements Document](../requirements/RabbitCare-MRD.pdf)
- [Product Requirements Document](../requirements/RabbitCare-PRD.pdf)
- [System Requirements Document](../requirements/RabbitCare-SRD.pdf)
- [Baseline Gap Analysis](../analysis/RabbitCare-Gap-Analysis.pdf)
- [PRD Refinement v1.2](../refinement/PRD_Refinement_v1.2.md)
- [Gap Analysis Refinement
  v1.2](../refinement/Gap_Analysis_Refinement_v1.2.md)
- [SRD Refinement v1.2](../refinement/SRD_Refinement_v1.2_REVISED.md)
- [Booking activity diagram
  v1.2](../../plantuml/refinement/05_activity_booking_service_v1.2.puml)
- [Payment activity diagram
  v1.2](../../plantuml/refinement/09_activity_payment_v1.2.puml)
- [Class diagram
  v1.2](../../plantuml/refinement/17_class_diagram_v1.2.puml)
- [Visit state diagram
  v1.2](../../plantuml/refinement/22_state_visit_v1.2.puml)
- [Service Execution state diagram
  v1.2](../../plantuml/refinement/23_state_service_execution_v1.2.puml)

## Outcome

The project produced a structured analysis package that connects
documented business problems to proposed product behavior, system-level
controls, and traceable models. The most important analytical result is
a clearer separation of booking, visit, service execution, invoice, and
payment responsibilities, supported by explicit booking-approval and
payment-verification rules.

This is a **documented system proposal**, not evidence of a deployed
application or measured operational improvement.

## Scope, assumptions, and open decisions

- The As-Is conditions are documented project assumptions and have not
  been independently verified with clinic stakeholders.
- Baseline documents and refinement addenda remain Draft / pending
  stakeholder validation.
- The application has not been implemented as part of this analysis
  repository.
- The payment gateway provider and provider-specific configuration are
  not yet selected.
- Pending-slot hold and expiry rules, the Admin-created booking
  workflow, and some Visit / Service Execution state transitions require
  stakeholder decisions.
- The UI prototype was created by another contributor; it may be shown
  only as product context, with attribution.

## Skills demonstrated

Business Process Analysis · Gap Analysis · SWOT and Fishbone Analysis ·
Requirements Engineering · Functional and Non-Functional Requirements ·
Acceptance Criteria · Requirements Traceability · UML / PlantUML System
Modeling · Transaction and Integration Analysis · Version-controlled
Documentation

------------------------------------------------------------------------

**Portfolio presentation note:** Lead with the As-Is vs To-Be
comparison, the Admin booking-approval flow, and the five-lifecycle
model. Use the payment-verification flow as a deeper technical-analysis
example. If the Figma Make prototype is shown, label it as a separate UI
artifact created by another contributor.

# RabbitCare Portfolio Asset Guide

This guide explains how to use the case study and its four presentation visuals in a 16:9 portfolio PDF or interactive portfolio website.

## Recommended presentation order

| Order | Visual | Main message | What to explain |
|---|---|---|---|
| 1 | `01_booking_process_comparison.png` | The refined proposal changes the booking decision flow | The baseline already includes Owner-initiated booking. The refinement adds a pending request, an Admin decision, and capacity revalidation. |
| 2 | `02_booking_approval_workflow.png` | The target booking workflow has explicit controls | Explain validation, `MENUNGGU_KONFIRMASI`, authorized Admin approval/rejection, decision metadata, and revalidation before confirmation. |
| 3 | `03_five_lifecycle_relationships.png` | Five status domains have separate responsibilities | Explain Booking, Visit, Service Execution, Invoice, and Payment Attempt; do not imply that one status automatically updates another. |
| 4 | `04_payment_verification_flow.png` | Payment settlement depends on a verified provider result | Explain idempotency, callback authentication, matching transaction data, duplicate-event handling, and reconciliation. |

## Suggested 16:9 portfolio structure

1. **Project introduction** — product concept, domain, your role, project status.
2. **Business analysis** — business context, baseline assumptions, and the baseline-to-To-Be comparison.
3. **Process analysis** — booking approval workflow and the reason for capacity revalidation.
4. **System analysis** — five lifecycle model and the separation of status responsibilities.
5. **Integration analysis** — payment verification and idempotency controls.
6. **Contribution and deliverables** — BRD, MRD, PRD, SRD, Gap Analysis, PlantUML models, and traceability.

Keep the slide text short. Let the diagram carry the explanation; use two or three supporting points rather than reproducing the full case study.

## Short interview introduction

> RabbitCare is a proposed rabbit-clinic management system. My contribution focused on Business and System Analysis: mapping the repository baseline, defining the Owner booking request and Admin approval workflow, refining BRD/MRD/PRD/SRD requirements, and modeling system behavior in PlantUML. One key analysis decision was separating Booking, Visit, Service Execution, Invoice, and Payment Attempt lifecycles. The proposal also defines backend verification for payment gateway callbacks. The UI prototype was created by another contributor, and the analysis remains pending stakeholder validation.

## Contribution boundary and claims

**Safe to present as your work, where supported by your actual contribution history:**
- Business process and gap analysis.
- Requirements documentation and refinement.
- Business rules, acceptance criteria, and traceability.
- System analysis and PlantUML modeling.
- Analysis of booking approval, lifecycle separation, and payment verification controls.

**Do not claim:**
- The Figma Make UI/UX design as your own; attribute it to the other contributor.
- Production implementation, deployment, or live payment-gateway integration.
- Measured operational improvements or user outcomes.
- Field-validated clinic conditions where no stakeholder or field evidence exists.
- Stakeholder approval or technical sign-off; the proposal remains Draft / pending validation.

## Visual asset notes

- PNG files are convenient for PDF, Canva, and slides.
- SVG files are scalable for web use and design tools that support SVG.
- Editable Graphviz `.dot` source files are included in `diagram_sources/`.
- These four visuals are **portfolio summary diagrams derived from the repository's analysis models**. They do not replace the original PlantUML sources under `plantuml/`.

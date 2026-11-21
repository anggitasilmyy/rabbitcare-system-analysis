# RabbitCare - PlantUML Refinement v1.1

This folder contains versioned PlantUML sources that refine the original v1.0 models. The original diagrams in the parent `plantuml/` folder are preserved and remain unchanged.

The confirmed booking model is **Owner self-service request submission followed by Admin approval/rejection**. Owner submission creates a pending request; it does not confirm the appointment. Payment gateway integration is included in the MVP, and payment status is updated only after verified provider results.

## Refined diagrams

| File | Purpose |
|---|---|
| `01_use_case_context_v1.1.puml` | System context, actors, Owner booking request, Admin decision, and gateway payment. |
| `02_use_case_owner_portal_v1.1.puml` | Owner self-service profile, booking request, status visibility, invoice, and payment interactions. |
| `03_use_case_clinic_operations_v1.1.puml` | Admin review queue, approval/rejection, capacity revalidation, clinic operations, and payment monitoring. |
| `05_activity_booking_service_v1.1.puml` | Owner booking request, pending status, Admin approval/rejection, and slot revalidation. |
| `09_activity_payment_v1.1.puml` | Invoice payment through a gateway, callback verification, duplicate handling, and receipt issuance. |
| `10_fishbone_gap_analysis_v1.1.puml` | Root-cause analysis aligned with owner request submission and Admin decision. |
| `11_sequence_booking_v1.1.puml` | Owner Portal, API, Booking/Schedule Services, database, Admin review, and notifications. |
| `15_sequence_payment_v1.1.puml` | Payment attempt, gateway checkout, callback verification, invoice update, and receipt. |
| `17_class_diagram_v1.1.puml` | Booking, separate Visit lifecycle, payment attempts, callback events, and receipt relationships. |
| `18_state_booking_v1.1.puml` | Booking request and decision lifecycle only. |
| `19_component_diagram_v1.1.puml` | Owner Portal, Clinic Portal, booking, schedule, billing, payment, and gateway components. |
| `20_deployment_diagram_v1.1.puml` | Application deployment and external payment gateway callback connection. |
| `21_state_payment_v1.1.puml` | Payment attempt lifecycle and verified status transitions. |
| `22_state_visit_v1.1.puml` | Visit attendance and service lifecycle, separate from Booking. |

## Modeling rules

- Owner submits a booking request through the Owner Portal.
- A submitted request is stored as `MENUNGGU_KONFIRMASI`; it is not a confirmed appointment.
- Only an authorized Admin can approve or reject an owner-submitted request.
- Approval requires slot-capacity revalidation.
- Pending slot hold/expiry policy remains subject to stakeholder validation.
- Booking lifecycle and Visit lifecycle are modeled separately.
- A `Payment` record represents one payment attempt; an invoice can have multiple attempts.
- A payment becomes `PAID` only after a verified provider result.
- Invoice and Payment status do not change Booking or Visit status.
- Provider-specific details remain open until a gateway provider is selected.
- A separate Admin-created booking workflow is not assumed; it remains a stakeholder-validation item.

## Baseline preservation

The original v1.0 `.puml` files in the parent folder are not overwritten. These v1.1 files are maintained as a separate refinement set.

**Status:** Proposed refinement / Draft - pending stakeholder validation.

# RabbitCare — PlantUML Refinement v1.2

This folder contains the v1.2 refinement sources. The original v1.0 diagrams and the previously committed v1.1 refinement files should remain unchanged. Add these v1.2 files alongside the existing versions in `plantuml/refinement/`.

## What changed in v1.2

1. Corrected the payment gateway communication direction: checkout/status inquiry is outbound from RabbitCare; callback/webhook is inbound from the provider.
2. Removed the incorrect use-case `include` relationship between Owner payment initiation and asynchronous provider callback verification.
3. Removed the direct Payment Gateway actor interaction from the Owner Portal use-case diagram. Callback verification is a backend-to-provider integration.
4. Added explicit checkout creation failure and timeout/unknown-outcome handling. A timeout keeps the attempt `PENDING`, requires status inquiry when supported or exception handling, and must not trigger a new attempt before the previous outcome is established.
5. Removed the assumption that any new idempotency key must be rejected while an active attempt exists. That policy remains an open stakeholder decision; the documented same-key idempotency behavior is retained.
6. Separated Visit lifecycle from Service Execution lifecycle in the class and state diagrams.
7. Added Pet-to-MedicalRecord traceability in the class diagram, consistent with the baseline patient-history requirement.
8. Added invoice-line traceability to recorded service executions as a proposed billing mapping; exact cardinality remains open.
9. Added a dedicated Service Execution state diagram.
10. Added booking creator identity, role snapshot, and creation-channel metadata to the booking activity, sequence, and class models.
11. Added explicit handling for verified-but-unsupported callback events and unmatched payment references without changing Payment/Invoice status or issuing a receipt.
12. Kept provider callback verification in the backend/system context rather than presenting it as a Clinic Portal action.
13. Removed the Payment Gateway actor and callback-verification use case from the Clinic Operations Portal detail; Admin/Manager monitoring and exception review remain.
14. Added booking creator user identity, role snapshot, and creation-channel metadata to the class model for booking-source traceability.
15. Aligned the Service Execution state diagram with the `DIBATALKAN` domain value for pre-start cancellation; cancellation after execution starts remains an open decision.
16. Revised the fishbone analysis to reflect that the baseline already includes Owner-initiated booking; the documented gap is the absence of an explicit pending-request/Admin-review workflow and related traceability.

## Diagram index

| File | Purpose |
|---|---|
| `01_use_case_context_v1.2.puml` | System context, actors, owner booking, admin decision, and backend gateway callback. |
| `02_use_case_owner_portal_v1.2.puml` | Owner Portal interactions; provider callback is explicitly outside this portal boundary. |
| `03_use_case_clinic_operations_v1.2.puml` | Clinic operations, admin booking review, and payment monitoring. |
| `05_activity_booking_service_v1.2.puml` | Owner booking request, pending status, admin approval/rejection, and slot revalidation. |
| `09_activity_payment_v1.2.puml` | Payment initiation, idempotency, gateway creation failure/timeout, duplicate/invalid/unsupported/unmatched callback handling, and receipt. |
| `10_fishbone_gap_analysis_v1.2.puml` | Root-cause analysis including distinct visit, service, invoice, and payment statuses. |
| `11_sequence_booking_v1.2.puml` | Owner booking request and admin decision sequence. |
| `15_sequence_payment_v1.2.puml` | Payment attempt, checkout result, timeout/inquiry, callback verification, and receipt. |
| `17_class_diagram_v1.2.puml` | Booking creator metadata, Booking/Visit/Service Execution lifecycles, medical-record traceability, payment attempts, and callback events. |
| `18_state_booking_v1.2.puml` | Booking request and admin decision lifecycle. |
| `19_component_diagram_v1.2.puml` | Corrected gateway directions and separate Visit/Service Execution components. |
| `20_deployment_diagram_v1.2.puml` | Application deployment and external payment gateway callback connection. |
| `21_state_payment_v1.2.puml` | Payment attempt lifecycle and verified status transitions. |
| `22_state_visit_v1.2.puml` | Visit attendance/encounter lifecycle only. |
| `23_state_service_execution_v1.2.puml` | Independent service execution lifecycle, including proposed pre-start cancellation. |
| `24_process_comparison_baseline_to_be_v1.2.puml` | Side-by-side comparison of the repository baseline booking model and the proposed v1.2 To-Be workflow. |

## Modeling rules and open decisions

- The baseline model already includes Owner-initiated booking and Owner confirmation. The v1.2 refinement changes the decision flow by creating a pending request for Admin review; it does not introduce self-service booking for the first time.
- Owner submission creates a booking request with status `MENUNGGU_KONFIRMASI`; it is not a confirmed appointment.
- Only an authorized Admin can approve or reject an owner-submitted request; approval requires capacity revalidation.
- Pending slot hold/expiry and the Admin-created booking workflow remain subject to stakeholder validation.
- One `Payment` record represents one payment attempt; an invoice can have multiple attempts.
- A repeated request with the same `Idempotency-Key` returns the existing attempt, consistent with the SRD API contract.
- The behavior for a different idempotency key while another attempt is active remains open; do not hard-code an active-attempt rejection before stakeholder decision.
- If gateway checkout creation times out or the outcome is unknown, keep the attempt `PENDING`, perform status inquiry if supported, or route it to exception handling. Do not create another attempt until the prior provider outcome is established.
- Payment becomes `PAID` only after a verified provider result. Invoice becomes `LUNAS` only after that verified success.
- Payment and Invoice do not change Booking, Visit, or Service Execution status.
- Visit and individual Service Execution statuses are separate. The exact state names/transitions shown in v1.2 are proposed modeling values pending stakeholder validation.
- `DIBATALKAN` is represented as a proposed Service Execution terminal state for cancellation before execution starts. Cancellation after execution starts remains undefined pending stakeholder validation.
- Fishbone causes are hypotheses from repository analysis, not verified field findings; validate them with clinic stakeholders before presenting them as observed facts.
- Pet medical history remains traceable directly through Pet ID, as specified by the baseline.
- Invoice items can retain source references to recorded service executions/items; exact mapping/cardinality remains a detailed billing-design decision.
- Provider-specific details remain open until a gateway provider is selected.

## Related requirement addenda

- [PRD Refinement v1.2](../../docs/refinement/PRD_Refinement_v1.2.md) clarifies the product lifecycle model and adds PR-66/PR-67.
- [Gap Analysis Refinement v1.2](../../docs/refinement/Gap_Analysis_Refinement_v1.2.md) separates Visit and Service Execution in the target-state analysis.
- [SRD Refinement v1.2](../../docs/refinement/SRD_Refinement_v1.2_REVISED.md) defines the corresponding system requirements and technical constraints.

## Baseline preservation

The original v1.0 `.puml` files and previously committed v1.1 refinement files are not overwritten. Keep this v1.2 set as a new version in `plantuml/refinement/`.

**Status:** Proposed refinement / Draft — pending stakeholder validation.

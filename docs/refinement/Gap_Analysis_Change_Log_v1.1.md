# RabbitCare Gap Analysis Change Log v1.1

**Baseline:** `docs/analysis/RabbitCare-Gap-Analysis.pdf`  
**Refinement date:** 21 November 2026  
**Status:** Draft for stakeholder validation

## Purpose

This change log records the changes introduced by Gap Analysis Refinement v1.1. The original Gap Analysis PDF is retained unchanged. The refinement aligns the target booking model with the BRD, MRD, PRD, and SRD v1.1 direction.

## Change Summary

| Area | Original v1.0 position | Refinement v1.1 |
|---|---|---|
| Booking request | The To-Be model describes owners submitting bookings through the system. | Owner self-service booking request submission is retained through the Owner Portal. |
| Booking confirmation | The baseline describes booking confirmation but does not define the approval decision in sufficient detail. | An owner-submitted request is stored as `MENUNGGU_KONFIRMASI`; an authorized admin must approve or reject it before it becomes confirmed. |
| Slot availability | The baseline identifies schedule conflict and double-booking risk. | The system checks availability at request submission and revalidates capacity during admin approval. Pending slot hold and expiry policy remain open for stakeholder validation. |
| Owner visibility | The baseline includes owner booking capability. | The owner can view the request status and rejection reason, if applicable, through the Owner Portal. |
| Admin responsibility | Admin manages clinic schedules and booking operations. | Admin receives a review queue and records approval/rejection decision metadata; this does not replace owner self-service request submission. |
| Admin-created booking | Not separately specified in the baseline target flow. | A separate workflow for bookings entered by Admin on behalf of an owner remains an open decision and is not treated as the confirmed owner-submitted booking flow. |
| Lifecycle statuses | Booking, visit, and payment labels appear together in parts of the original analysis. | Booking, visit/service, invoice, and payment attempt lifecycles are separated. |
| Payment gateway | Payment methods and payment evidence are discussed; gateway integration is not a core MVP commitment in the baseline. | Payment gateway integration is included in the MVP, with server-side callback verification, reference/amount matching, idempotency, and reconciliation considerations. |
| SWOT | Gateway integration is not positioned as an MVP capability. | Gateway onboarding, callback verification, provider dependency, and reconciliation readiness are treated as MVP implementation risks. |
| Fishbone | General manual-process and technology causes are identified. | Owner request submission, admin review, capacity validation, gateway callback, and transaction-traceability causes are made explicit. |
| Priority | Booking and payment are high-priority process gaps. | Owner self-service request, admin approval, capacity validation, and gateway-based payment are treated as high/very-high MVP gaps. |
| Roadmap | Booking is included in the target system; payment gateway is not a confirmed MVP dependency. | Owner request and admin decision workflow and gateway-based payment are included in the MVP roadmap. |
| Success indicators | The original target includes a high proportion of bookings made through the system. | Booking success is measured through owner-submitted requests, traceable admin decisions, owner status visibility, and no confirmed over-capacity slots. |

## Baseline Preservation

- The original `RabbitCare-Gap-Analysis.pdf` remains in `docs/analysis/` and is not edited.
- This addendum is stored separately in `docs/refinement/`.
- Refined PlantUML sources are stored separately in `plantuml/refinement/`.
- Original v1.0 PlantUML diagrams are preserved in the root `plantuml/` folder.
- No original analysis file or original PlantUML diagram is deleted or overwritten.

## Validation Notes

The following items remain stakeholder-validation points: admin approval SLA, exact staff/service slot-capacity rules, pending slot hold and expiry policy, any separate admin-entry workflow for phone/walk-in requests, gateway provider and supported methods, refund and outage procedures, notification channels, and pilot success thresholds.

**Status:** Draft for stakeholder validation.

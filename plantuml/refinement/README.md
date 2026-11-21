# RabbitCare - PlantUML Refinement v1.1

This folder contains refined PlantUML sources for the booking-admin-approval and payment-gateway MVP decisions.

The original v1.0 diagrams remain in the parent `plantuml/` folder and are not overwritten. These v1.1 sources are maintained as a separate refinement set.

## Refined diagrams

| File | Purpose |
|---|---|
| `05_activity_booking_service_v1.1.puml` | Owner booking request, admin approval/rejection, and slot revalidation. |
| `09_activity_payment_v1.1.puml` | Invoice payment through a gateway and verified callback outcome. |
| `11_sequence_booking_v1.1.puml` | Booking API, database, admin decision, and notifications. |
| `15_sequence_payment_v1.1.puml` | Payment attempt, gateway checkout, callback verification, invoice update, and receipt. |
| `17_class_diagram_v1.1.puml` | Booking decision metadata, payment attempts, callback events, and receipt relationships. |
| `18_state_booking_v1.1.puml` | Booking operational lifecycle separated from payment. |
| `19_component_diagram_v1.1.puml` | Booking, billing, payment, gateway adapter, and notification components. |
| `20_deployment_diagram_v1.1.puml` | Application deployment and external payment gateway callback connection. |
| `21_state_payment_v1.1.puml` | Payment attempt lifecycle and verified status transitions. |

## Design notes

- `MENUNGGU_KONFIRMASI` is a request state, not a confirmed booking.
- Only an authorized admin can approve or reject a booking.
- Slot capacity is revalidated during approval.
- A `Payment` record represents one payment attempt; an invoice can have multiple attempts.
- A payment becomes `PAID` only after a verified provider result.
- Booking status and payment/invoice status are separate lifecycles.
- Provider-specific details remain open until a gateway provider is selected.

**Status:** Proposed refinement / Draft - pending stakeholder validation.

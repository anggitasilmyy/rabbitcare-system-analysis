# RabbitCare Gap Analysis Refinement v1.1

**Project:** RabbitCare — Rabbit Clinic & Care Management System  
**Document type:** Gap Analysis Refinement Addendum  
**Version:** 1.1  
**Refinement date:** 21 November 2026  
**Baseline:** `docs/analysis/RabbitCare-Gap-Analysis.pdf` (19 pages; original analysis)  
**Status:** Draft for stakeholder validation  
**Language:** English repository documentation

---

## Document Control

| Item | Description |
|---|---|
| Purpose | Refine the original gap analysis to align it with the latest product decisions and the BRD, MRD, PRD, and SRD refinement direction |
| Baseline preservation | The original Gap Analysis PDF remains unchanged in `docs/analysis/` |
| Scope | Booking, staff scheduling, service operations, billing, payment, reporting, access control, infrastructure, and user adoption |
| Confirmed decisions | Owner-submitted booking is self-service at request stage and requires admin approval/rejection before confirmation; payment gateway integration is included in the MVP |
| Versioning | This addendum records refinements; it does not silently rewrite the original analysis |

## 1. Purpose and Scope

This addendum refines the original RabbitCare Gap Analysis by updating the target operating model, gap priorities, SWOT implications, root-cause analysis, and implementation roadmap.

The original document identifies 18 process areas and describes the current state as largely manual, with information distributed across notebooks, spreadsheets, separate documents, telephone calls, and messaging applications. The original document also states that the As-Is conditions are assumed characteristics and should therefore be validated with clinic stakeholders before implementation.

The refinement preserves the original problem context while aligning the proposed solution with two confirmed product decisions:

1. **Owner-submitted booking requires admin approval.** Owners can submit booking requests through the Owner Portal. A submitted request is not a confirmed appointment; an authorized admin must review and approve or reject it. The system must revalidate slot capacity before approval.
2. **Payment gateway is part of the MVP.** Gateway integration is no longer positioned as a long-term enhancement. The MVP must support a gateway-based payment flow and verified transaction status.

This document is a refinement addendum, not evidence that the system has already been implemented or that the clinic's current operations have been independently audited.

## 2. Confirmed Product Decisions


### 2.1 Owner-Submitted Booking with Admin Approval

The Owner Portal allows rabbit owners to submit appointment requests through the application. Submitting a request does not mean that the appointment is automatically confirmed. An authorized clinic admin must review the request, check staff availability and service capacity, and approve or reject it.

**Target flow**

1. Owner selects a rabbit profile, service, preferred date, and available time slot displayed through the Owner Portal.
2. System validates the submitted information and performs an initial availability and capacity check.
3. System generates a booking reference and stores the request with status `MENUNGGU_KONFIRMASI`.
4. The pending request becomes available in the admin review queue.
5. Admin reviews the request, staff schedule, and service capacity.
6. Before approval, the system revalidates slot capacity to prevent overlapping confirmed bookings.
7. If capacity is available, the admin approves the request and the status changes to `TERKONFIRMASI`.
8. If the request is rejected, the status changes to `DITOLAK`, and the rejection reason, admin identity, and decision timestamp are recorded.
9. The system notifies the owner about the submission and the final decision.

The owner can submit a booking request but cannot directly confirm an appointment. The confirmation decision remains under authorized clinic admin control.

Additional booking channels such as WhatsApp, telephone, or walk-in requests may be considered separately. Their integration into the RabbitCare workflow requires stakeholder validation.

The pending-slot hold duration and expiry policy remain open decisions and must not be assumed as finalized business rules.


### 2.2 Booking, Visit, Invoice, and Payment Are Separate Lifecycles

The original analysis groups several service and payment labels in one status list. The refinement separates them so that a booking status cannot be mistaken for a visit status or payment status.

| Lifecycle | Proposed statuses | Meaning |
|---|---|---|
| Booking | `MENUNGGU_KONFIRMASI`, `TERKONFIRMASI`, `DITOLAK`, `DIBATALKAN`, `DIJADWALKAN_ULANG` | Owner-submitted appointment request and admin decision |
| Visit/service | `BELUM_CHECK_IN`, `CHECKED_IN`, `MENUNGGU_ANTREAN`, `SEDANG_DILAYANI`, `SELESAI_DILAYANI`, `TIDAK_HADIR` | Physical attendance and service progress |
| Invoice | `BELUM_DIBAYAR`, `LUNAS` | Amount due and settlement state |
| Payment attempt | `PENDING`, `PAID`, `FAILED`, `CANCELLED` | Individual gateway transaction attempt |

These are logical lifecycle groupings. Exact database enums, transition permissions, and exception states must remain consistent with the approved SRD and be validated before implementation.

### 2.3 Payment Gateway in the MVP

The MVP includes integration with one payment gateway selected by the clinic/project. The gateway provider is not specified in this document.

**Target payment flow**

1. Staff completes the service record and generates an invoice.
2. The system creates a payment request with a unique internal reference and the expected amount.
3. The backend submits the payment request to the configured gateway.
4. The owner completes payment using the gateway's available payment methods.
5. The backend receives the provider's callback/webhook.
6. The backend verifies callback authenticity and matches the provider transaction, internal reference, amount, and payment status.
7. Only a verified successful payment changes the payment attempt to `PAID` and the invoice to `LUNAS`.
8. The system stores the transaction reference and issues or makes available a receipt.

A browser redirect or client-side success message alone must not be treated as proof of payment. Duplicate callbacks must not create duplicate settlement records. Failed, cancelled, or pending attempts must not mark an invoice as paid.

The gateway provider, supported payment methods, expiry rules, refund handling, settlement/reconciliation procedure, and outage fallback must be confirmed with stakeholders. This addendum does not assume a specific provider or invent a manual-payment fallback policy.

## 3. Refined As-Is and To-Be Summary

### 3.1 As-Is Baseline

The original analysis describes the following assumed conditions:

- Booking requests are received through WhatsApp, telephone, or walk-in visits.
- Doctor and groomer schedules are recorded manually.
- Owner and rabbit data are distributed across notebooks, spreadsheets, and separate documents.
- Medical and grooming histories are not consistently centralized.
- Check-in, queue, reminders, billing, and reporting rely on manual activities.
- Payment evidence is stored manually.
- Role-based access and audit trails are not systematically implemented.

These are baseline assumptions from the original document, not independently verified findings.

### 3.2 Refined To-Be

The refined target state is:

- Owners submit booking requests through the Owner Portal.
- The system validates the request and displays available schedule/slot options before submission.
- Submitted requests remain `MENUNGGU_KONFIRMASI` until an authorized admin approves or rejects them.
- Admin reviews the request and can check staff schedules and service capacity.
- The system revalidates capacity before approval to reduce double booking.
- Owners can view the booking decision and rejection reason, if applicable, in the Owner Portal.
- Each rabbit has a unique profile and medical record identifier.
- Visit, service, invoice, and payment statuses are tracked separately.
- The system creates invoices from recorded services and items.
- Payment is processed through a configured gateway in the MVP.
- Payment success is based on verified server-side provider notification, not solely on the owner's browser redirect.
- Transaction references and payment events are stored for audit and reconciliation.
- Role-based access, audit logging, operational reporting, and user training support implementation.

## 4. Refined Gap Analysis Matrix

The 18 original process areas are retained. The target state and priority are updated where necessary to reflect the confirmed product decisions.

| No. | Process area | As-Is baseline | Refined To-Be | Main gap / impact | Priority |
|---:|---|---|---|---|---|
| 1 | Owner registration | Owner data is recorded manually or through messages | Admin can register and maintain owner records in a centralized system | No consistent digital registration and duplicate-data control | High |
| 2 | Rabbit profile | Rabbit data is spread across separate records | Each rabbit has a centralized profile and unique ID | Patient history is difficult to trace | Very High |
| 3 | Booking | Requests arrive through WhatsApp, phone, or walk-in | Owners submit booking requests through the Owner Portal; admin reviews and approves/rejects requests after capacity revalidation | No structured request tracking, admin decision workflow, or capacity revalidation | Very High |
| 4 | Staff scheduling | Doctor and groomer schedules are recorded manually | Admin views and maintains staff availability and service capacity | Schedule conflicts and double-booking risk | Very High |
| 5 | Check-in | Arrival is recorded manually | Admin records arrival against a confirmed booking | Attendance status cannot be monitored consistently | High |
| 6 | Queue | Queue is communicated verbally | System generates and tracks queue order after check-in | Service flow is difficult to monitor | High |
| 7 | Medical record | Examination notes are separate or paper-based | Medical records are stored digitally and linked to the rabbit profile | Clinicians may not see complete patient history | Very High |
| 8 | Vaccination | Vaccination records and next dates are handled manually | Vaccination history and follow-up dates are recorded in the system | Vaccination follow-up may be missed | High |
| 9 | Grooming | Grooming results are not always documented | Grooming notes and findings are linked to the rabbit profile | Findings may not be followed up | High |
| 10 | Groomer referral | Referral is communicated verbally to the owner | Groomer records a referral for doctor review | Findings may be lost or not traceable | High |
| 11 | Invoice and billing | Charges are calculated from service notes | Invoice is generated from recorded services and applicable items | Manual calculation can cause errors and incomplete transaction records | Very High |
| 12 | Payment | Payment evidence is stored manually | MVP uses a configured gateway; verified payment events update payment and invoice status | No gateway integration, server-side verification, or centralized reconciliation | Very High |
| 13 | Reminder | Staff sends reminders manually | System schedules reminders from recorded follow-up needs | Control and care schedules may be missed | High |
| 14 | Reporting | Data is recompiled manually | Manager can access operational reports from structured data | Evaluation takes additional time | Medium |
| 15 | Access control | Staff access is not systematically differentiated | Access is assigned by role and responsibility | Unauthorized access or data exposure risk | Very High |
| 16 | Audit trail | Changes are not consistently recorded | Important changes and payment events are logged with actor and timestamp | Difficult to identify who changed data or payment status | High |
| 17 | Infrastructure | Operations depend on devices and manual processes | Web application, centralized database, secure backend, and gateway connectivity support the MVP | Hosting, availability, security, backup, and provider dependency must be managed | High |
| 18 | User competency | Staff are accustomed to manual processes | Staff receive role-based guidance, training, and pilot support | Adoption problems and data-entry errors may delay implementation | High |

## 5. Refined Gap Categories

### 5.1 Data Integration Gap

Owner, rabbit, booking, service, medical, invoice, and payment information must be linked through a centralized data model. Unique identifiers are needed to reduce duplicate records and improve traceability.

### 5.2 Booking and Service-Control Gap

The target combines owner self-service request submission with admin-controlled confirmation. Owners submit booking requests through the Owner Portal and can see available schedule options. The system records each valid request as `MENUNGGU_KONFIRMASI`; an authorized admin reviews it and approves or rejects it. Capacity is revalidated during approval, and the decision, actor, timestamp, and rejection reason (when applicable) are traceable.

The system must not treat request submission as confirmation. Booking status must remain separate from check-in, queue, and service progress. The policy for any additional admin-created booking workflow remains open for stakeholder validation.

### 5.3 Continuity-of-Care Gap

Examination, vaccination, grooming, medication, and follow-up information must be linked to the rabbit profile. Reminder scheduling should use documented follow-up dates and responsible users.

### 5.4 Billing and Payment-Control Gap

Billing must be generated from recorded services and items. The MVP payment gateway integration must provide:

- A unique internal invoice/payment reference.
- A gateway transaction reference.
- Server-side callback/webhook verification.
- Amount and transaction matching.
- Idempotent handling of repeated callbacks.
- Clear pending, paid, failed, and cancelled payment-attempt states.
- Invoice settlement only after verified success.
- Transaction records that support audit and reconciliation.

### 5.5 Access, Audit, and Operational-Visibility Gap

Role-based access, audit logging, and operational reports are required to make data changes traceable and to support management decisions.

### 5.6 Infrastructure and Adoption Gap

The MVP depends on reliable hosting, database backup, secure gateway communication, provider availability, and staff readiness. Training and pilot testing remain necessary even when the application is technically ready.

## 6. SWOT Refinement

The original SWOT structure is retained. The following changes are required to align it with the updated MVP scope.

### 6.1 Strengths

- RabbitCare focuses on rabbit clinic and care operations.
- Medical, vaccination, grooming, billing, and reminder processes are designed to be connected.
- The project reflects an understanding of clinic workflows.
- Rabbit profiles and service history are intended to be centralized.
- Role-based access supports separation of responsibilities.
- Rabbit-specific pre-vaccination and grooming checks can differentiate the system.

### 6.2 Weaknesses

- The product remains at the design/refinement stage and has not been proven through full operational deployment.
- Successful use depends on accurate and timely staff input.
- The initial scope focuses on one clinic.
- Laboratory and inpatient modules are outside the current MVP scope.
- Payment gateway integration is now in the MVP, so implementation depends on provider onboarding, technical integration, callback verification, and reconciliation readiness.
- Hosting, maintenance, backup, and internet connectivity require operational planning.
- Historical data may need cleaning and migration.

The previous statement that payment gateway integration is not a main feature must be removed because it conflicts with the confirmed MVP decision.

### 6.3 Opportunities

- Adoption by other rabbit clinics.
- Subscription-based clinic software in a later phase.
- Multi-branch capability.
- WhatsApp or other channels for reminders.
- Partnerships with rabbit communities.
- Nutrition, weight tracking, mobile, loyalty, and teleconsultation enhancements in later phases.

### 6.4 Threats

- User resistance to changed workflows.
- Competition from general veterinary clinic systems.
- Internet, server, or hosting outages.
- Notification or payment-provider downtime.
- Delayed, duplicated, invalid, or mismatched gateway callbacks.
- Data leakage or misuse.
- Provider fees or changes to provider terms.
- Sudden changes in doctor schedules.
- Inaccurate medical or transaction data.

## 7. Refined SWOT Strategies

| Strategy | Updated direction |
|---|---|
| SO | Use rabbit-specific workflows and integrated patient history as the differentiator; retain an extensible data structure for future clinic expansion |
| ST | Apply role-based access, audit logs, input validation, verified gateway callbacks, and transaction matching to reduce security and transaction risks |
| WO | Conduct a controlled pilot, train clinic staff, validate gateway integration in the provider sandbox, and prioritize MVP gaps before adding advanced modules |
| WT | Define procedures for internet or provider outages, monitor failed payment attempts, prevent duplicate settlement, and retain traceable exception handling |

## 8. Refined Fishbone Analysis

**Core problem:** Rabbit Clinic and Care services are not yet integrated, operational work is inefficient, and booking, payment, or follow-up information may be missed or become difficult to trace.

| Cause category | Refined contributing causes |
|---|---|
| People | Owners may submit incomplete booking details; admins may delay reviewing pending requests; staff may enter incomplete transaction data; digital skill levels vary; responsibility for decision updates may be unclear |
| Process | Owner request submission and admin decision workflow may not be standardized; capacity must be revalidated before approval; slot hold/expiry policy remains open; visit and payment statuses may be mixed; payment reconciliation may be manual |
| Technology | No integrated Owner Portal booking request flow in the current state; no admin review queue; no structured slot-capacity validation; no gateway callback verification; no integrated notification and reporting services |
| Data | Owner and rabbit records may be duplicated; medical history is fragmented; booking and invoice references may not be consistently linked; payment evidence may be difficult to trace |
| Management and Measurement | No consolidated operational dashboard; booking, no-show, service-time, and payment reconciliation indicators may not be consistently measured |
| Environment and External Services | Internet or hosting outages; gateway and notification-provider downtime; provider callback delays; changes in staff schedules; limited devices |

The updated PlantUML source is provided separately at `plantuml/refinement/10_fishbone_gap_analysis_v1.1.puml`.

## 9. Root-Cause Priority

| Rank | Root cause / gap | Reason for priority |
|---:|---|---|
| 1 | No centralized and linked data foundation | Required for owner, rabbit, booking, service, invoice, and payment traceability |
| 2 | No integrated owner request and admin approval workflow with capacity validation | Directly addresses request visibility, decision traceability, and double-booking risk |
| 3 | Medical history is not integrated | Affects continuity and quality of clinical service |
| 4 | Billing and gateway payment are not integrated | Payment gateway is a confirmed MVP capability and must support verified settlement |
| 5 | Service and visit statuses are not standardized | Needed for check-in, queue, and service monitoring |
| 6 | Reminder handling is manual | Follow-up and care schedules may be missed |
| 7 | Role-based access and audit are incomplete | Increases data and accountability risks |
| 8 | Reporting is manual | Slows operational evaluation |
| 9 | Users may not be ready for the new workflow | Can reduce adoption and data quality |
| 10 | External provider and infrastructure dependencies | Can interrupt payment, notification, or system availability |

## 10. Refined Implementation Roadmap

### 10.1 MVP — Short Term

The MVP should include:

- Owner and rabbit registration/profile management.
- Owner self-service booking request submission through the Owner Portal.
- Admin review queue and approval/rejection workflow for owner-submitted requests.
- Staff schedule and service-capacity checking.
- Booking status visibility and capacity revalidation before approval.
- Check-in and queue management.
- Medical records and basic vaccination records.
- Grooming records and referral notes.
- Invoice generation from recorded services/items.
- Payment gateway integration with server-side transaction verification.
- Payment and invoice status separation.
- Transaction reference storage and receipt availability.
- Basic follow-up reminder capability.
- Role-based access for the defined user roles.

### 10.2 Medium Term

- More complete audit-log views and operational monitoring.
- Management dashboard and visit/revenue reports.
- Notification retry and delivery monitoring.
- Historical patient-data migration improvements.
- Weight tracking and additional care analytics.
- Improved exception handling based on pilot feedback.

### 10.3 Long Term

- Multi-branch support.
- Dedicated mobile application, if justified.
- Teleconsultation.
- Inpatient and laboratory modules.
- Loyalty program.
- Pet-shop or external service partnerships.
- Advanced business analytics and device integrations.

**Roadmap correction:** Payment gateway is removed from the long-term list because it is part of the MVP. The long-term roadmap may include additional payment providers or advanced payment capabilities only if later justified.

## 11. Gap Closure Plan

| Gap | Action | Expected output |
|---|---|---|
| Owner and rabbit data are fragmented | Create centralized owner and rabbit records with unique identifiers | Linked patient information |
| Booking requests are handled manually across channels | Provide Owner Portal request submission, pending status, and admin approval/rejection | Owner-submitted requests are traceable from submission through admin decision |
| Schedule conflicts may occur | Add staff availability and service-capacity checks, including revalidation before confirmation | Lower double-booking risk |
| Visit progress is unclear | Link check-in, queue, and service statuses to the booking/visit record | Traceable service progress |
| Medical and grooming records are separate | Link clinical and grooming notes to the rabbit profile | More complete care history |
| Billing is calculated manually | Generate invoice from recorded services and items | More consistent billing |
| Payment is manually evidenced | Integrate the selected gateway and verify provider callbacks server-side | Traceable payment settlement |
| Duplicate or mismatched payment events may occur | Match references and amounts; process callbacks idempotently | Reduced duplicate-settlement risk |
| Reminder is manual | Add scheduling based on documented follow-up dates | More consistent follow-up |
| Access and changes are not traceable | Implement role-based access and audit logging | Better accountability |
| Reports require manual rework | Provide operational and revenue reports | Faster management review |
| Staff may not be ready | Run role-based training and a controlled pilot | Improved adoption and data quality |

## 12. Proposed Success Indicators

The original indicators are retained where relevant and adjusted to match owner-submitted booking with admin approval and gateway-based payment. Targets below are proposed acceptance targets and must be validated with clinic stakeholders.

1. At least 80% of booking requests in the pilot measurement period are submitted through the Owner Portal, subject to validation of the original baseline target with clinic stakeholders.
2. Every valid booking request submitted through the Owner Portal is stored in RabbitCare with a unique booking reference.
3. Every rabbit profile has a unique patient identifier.
4. No owner-submitted booking is confirmed without an authorized admin decision and successful capacity revalidation.
5. Owners can view the latest status of their own booking requests and the rejection reason when applicable.
6. No overlapping confirmed booking is created for the same constrained staff/resource slot.
7. At least 90% of completed examinations are stored digitally during the pilot measurement period.
8. Every visit has a traceable check-in and service status.
9. Follow-up reminders are generated from recorded care dates according to the agreed rules.
10. Every invoice has a unique internal reference.
11. Every successful gateway payment is matched to the correct invoice and amount before the invoice becomes `LUNAS`.
12. Repeated provider callbacks do not create duplicate settlement records.
13. Authorized clinicians can access the rabbit's relevant history before examination.
14. Grooming findings and referrals can be traced to the relevant rabbit profile.
15. Managers can access agreed operational reports without manual re-aggregation.
16. Users can access only the features and records permitted by their roles.

## 13. Risks, Dependencies, and Controls

| Risk / dependency | Potential impact | Proposed control |
|---|---|---|
| Owner submits incomplete booking details | Request cannot be reviewed or may contain invalid service/schedule information | Validate required fields, ownership, service eligibility, and selected schedule before accepting the request |
| Admin does not review a pending booking promptly | Owner waits for a decision and requested capacity may change | Provide a pending review queue; agree an approval SLA and escalation process with stakeholders |
| Staff schedule changes after request entry | Confirmed slot may become unavailable | Revalidate capacity before confirmation and define reschedule handling |
| Gateway is unavailable | Owner cannot complete payment through the provider | Define an approved outage and exception procedure before launch |
| Callback is delayed or duplicated | Payment status may be temporarily inaccurate or processed more than once | Idempotent callback handling, transaction lookup, and reconciliation |
| Callback is invalid or mismatched | Incorrect invoice settlement | Verify signature/authenticity, reference, amount, and provider status server-side |
| Provider onboarding or credentials are delayed | MVP integration is blocked | Confirm provider selection, account setup, and sandbox access early |
| Staff enter incomplete service or invoice data | Incorrect invoice or incomplete records | Required fields, validation, and role-based review |
| Internet or hosting outage | Staff cannot access the system | Backup, monitoring, and documented operational contingency |
| Staff resist the new process | Low adoption and inconsistent records | Training, simple workflows, and pilot feedback |

## 14. Items Requiring Stakeholder Validation

The following items are not specified by the original Gap Analysis or the two confirmed product decisions. They must be validated rather than assumed:

1. Should the Owner Portal display only available dates and time slots, or also include staff names and detailed service availability?
2. Which admin roles may approve, reject, cancel, or reschedule owner-submitted bookings?
3. What approval SLA and escalation process should apply to pending requests?
4. Are phone, walk-in, or other offline booking requests also accepted, and if so, should a separate admin-entry workflow be supported?
5. What are the clinic's slot-capacity, pending hold, and expiry rules for each service and staff member?
6. Which payment gateway provider will be used?
7. Which payment methods and transaction-expiry rules are required in the MVP?
8. What is the approved procedure for refunds, failed payments, gateway downtime, and manual exception handling?
9. What receipt fields and invoice numbering rules are required?
10. Which reminder channels and delivery rules are required for the MVP?
11. Which operational indicators and target thresholds will be used during the pilot?

## 15. Conclusion

The original Gap Analysis correctly identifies integration, booking, patient-history, reminder, billing, access-control, infrastructure, and adoption challenges. The refinement preserves those findings while updating the target model to reflect the current product direction.

The most important booking refinement is that owners retain self-service request submission through the Owner Portal, while an authorized admin must approve or reject each owner-submitted request before it becomes confirmed. Payment gateway integration is included in the MVP. Booking, visit, invoice, and payment lifecycles are separated to improve process clarity and traceability.

Successful gap closure requires more than application development. It also depends on clear admin responsibilities, reliable schedule and capacity validation, secure gateway integration, transaction reconciliation, role-based procedures, staff training, and a controlled pilot.

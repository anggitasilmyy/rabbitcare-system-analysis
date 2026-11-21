# RabbitCare Gap Analysis Change Log v1.1

**Baseline:** `docs/analysis/RabbitCare-Gap-Analysis.pdf`  
**Refinement date:** 21 November 2026  
**Status:** Draft for stakeholder validation

## Purpose

This change log records the changes introduced by Gap Analysis Refinement v1.1. The original PDF is retained unchanged.

## Change Summary

| Area | Original v1.0 position | Refinement v1.1 |
|---|---|---|
| Booking model | Owner self-service booking is described as the target | Booking is admin-managed; admin records the request and approves/rejects after availability and capacity checks |
| Booking confirmation | Confirmation workflow is not sufficiently defined | Booking begins as `MENUNGGU_KONFIRMASI`; authorized admin decision and capacity revalidation are required before `TERKONFIRMASI` |
| Lifecycle statuses | Booking, visit, and payment labels appear together | Booking, visit/service, invoice, and payment attempt lifecycles are separated |
| Payment gateway | Described as not a main feature and placed in long-term roadmap | Payment gateway integration is part of the MVP |
| Payment verification | Manual evidence and centralized history are emphasized | Server-side callback/webhook verification, reference and amount matching, idempotency, and reconciliation are added |
| SWOT | Gateway integration is listed as a weakness because it is not a main feature | Gateway becomes an MVP dependency and integration risk |
| Fishbone | General manual-process and technology causes | Admin booking workflow, capacity validation, gateway callback, and transaction-traceability causes are added |
| Priority | Payment and booking are high-priority gaps | Admin-managed booking and gateway-based payment are treated as very high-priority MVP gaps |
| Roadmap | Payment gateway is long term | Payment gateway is moved into MVP |
| Success indicators | Booking target can be interpreted as owner self-service | Booking success is measured by recording clinic booking requests and admin-controlled confirmation |
| Risks | External providers are discussed generally | Gateway downtime, delayed/duplicate callbacks, invalid callbacks, amount mismatch, and outage procedure are specified |

## Baseline Preservation

- The original `RabbitCare-Gap-Analysis.pdf` remains in `docs/analysis/`.
- This addendum is stored separately in `docs/refinement/`.
- The updated Fishbone PlantUML source is stored separately in `plantuml/refinement/`.
- No original analysis file or original PlantUML diagram is deleted or overwritten.

## Validation Notes

The gateway provider, supported payment methods, refund and outage procedures, booking intake channels, admin permissions, slot-capacity rules, and reminder channels remain stakeholder-validation items.

# RabbitCare — SRD Change Log v1.1

**Tanggal:** 21 November 2026  
**Baseline:** RabbitCare System Requirements Document v1.0 · Draft  
**Status:** Proposed refinement / Draft — pending stakeholder validation

## Keputusan produk yang diterjemahkan

| ID | Keputusan | Dampak teknis |
|---|---|---|
| DEC-01 | Booking memerlukan admin approval/rejection. | Booking request disimpan sebagai `MENUNGGU_KONFIRMASI`; admin mengambil keputusan; slot divalidasi ulang saat approval. |
| DEC-02 | Payment gateway termasuk MVP. | Payment attempt, provider reference, callback verification, payment/invoice state, receipt, idempotency, dan monitoring ditetapkan dalam SRD addendum. |

## Perubahan terhadap baseline SRD v1.0

| Bagian baseline | Refinement v1.1 |
|---|---|
| Section 3–5: System Context / Logical Architecture | Perjelas Admin Review, Payment Gateway Adapter, Callback Handler, dan Receipt Service. |
| SR-F-019–027 | Tambahkan pending approval, admin approval/rejection, decision metadata, slot revalidation, dan event notification. |
| API 7.3 | Pertahankan POST/GET/confirm/cancel/reschedule; tambahkan pending queue dan reject endpoint. |
| SR-F-058–066 | Tambahkan payment attempt gateway, reference, callback verification, idempotency, verified receipt, dan pemisahan booking/payment state. |
| API 7.8 | Pertahankan POST/GET payment dan receipt; tambahkan gateway callback endpoint. |
| Section 9 Data Requirements | Tambahkan booking decision metadata dan Payment Callback Event; jelaskan Payment sebagai attempt-level record. |
| Section 10 Data Validation | Tambahkan validation reference, amount, currency, callback, dan decision reason. |
| Section 12 Authorization Matrix | Admin approve/reject booking; Admin monitor payment; Owner initiate/read own payment. |
| Section 13 Transaction Rules | Pisahkan booking request, booking approval, rejection, payment initiation, dan verified callback transaction. |
| Section 14 Error Codes | Tambahkan booking decision dan payment gateway/callback error codes. |
| Section 15 Integration Requirements | Perjelas gateway adapter, callback verification, reference, amount, currency, dan idempotency. |
| Section 17 Logging and Monitoring | Tambahkan booking decision dan callback verification/processing logs. |
| Section 18 Deployment Requirements | Tambahkan HTTPS integration dan webhook endpoint gateway. |
| Section 19 Testing Requirements | Tambahkan concurrency, callback security, amount/reference mismatch, duplicate callback, dan payment regression tests. |
| Section 20 Traceability Matrix | Tambahkan mapping PR-56–PR-65 ke SR-F-085–SR-F-100. |
| Section 21 System Acceptance Criteria | Tambahkan approval/rejection dan gateway payment acceptance criteria. |

## Requirement IDs

- Baseline SR-F-001 sampai SR-F-084 dipertahankan.
- Baseline SR-NF-001 sampai SR-NF-034 dipertahankan.
- Requirement tambahan: SR-F-085 sampai SR-F-100.
- Nonfunctional requirement tambahan: SR-NF-035 sampai SR-NF-039.

## Diagram refinement

Diagram baseline di `plantuml/` tidak diubah. Versi refinement disimpan terpisah di `plantuml/refinement/`:
- Activity booking dan payment.
- Sequence booking dan payment.
- Class diagram.
- Booking state dan payment state.
- Component dan deployment diagram.

## Open decisions

Provider gateway, metode pembayaran, merchant account, slot hold/expiry, SLA approval, pembayaran manual sebagai alternatif, retry, reconciliation, refund, deposit, partial payment, dan target operasional provider masih menunggu keputusan.

## Preservasi baseline

PDF SRD v1.0 dan PlantUML baseline tetap dipertahankan. Addendum ini mencatat perubahan teknis secara terpisah agar versi asli tetap dapat ditelusuri.

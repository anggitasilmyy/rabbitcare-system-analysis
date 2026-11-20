# RabbitCare - PRD Change Log v1.1

**Tanggal:** 21 November 2026  
**Baseline:** Product Requirements Document v1.0 - MVP 1.0 / Draft  
**Status:** Proposed refinement / Draft - pending stakeholder validation

## Keputusan produk

| ID | Area | Keputusan | Dampak |
|---|---|---|---|
| DEC-01 | Booking | Booking memerlukan keputusan admin. | Pengajuan pemilik menunggu approval/rejection sebelum terkonfirmasi. |
| DEC-02 | Payment | Payment gateway termasuk MVP. | Pembayaran invoice melalui gateway, status transaksi terverifikasi, dan bukti pembayaran masuk MVP. |

## Perubahan pada bagian PRD v1.0

| Bagian | Perubahan v1.1 |
|---|---|
| 4 Product Goals | PG-02 diperjelas sebagai pengajuan mandiri dengan keputusan admin. |
| 6 Product Users | Peran admin mencakup review booking dan monitoring transaksi gateway. |
| 7 Product Scope | Owner status keputusan dan gateway payment; admin approval/rejection dan monitoring transaksi. |
| 8.2 Booking Journey | Ditambahkan status pending approval, admin decision, rejection reason, dan notifikasi hasil. |
| 8.5 Payment Journey | Pembayaran invoice melalui gateway; hasil diverifikasi sebelum paid/receipt. |
| 9.3 PR-13, PR-15 | Booking menjadi pengajuan dan admin dapat approve/reject. |
| 9.8 PR-43, PR-44, PR-45 | Payment admin diselaraskan dengan gateway; receipt setelah verified paid; payment state dipisah dari booking state. |
| 9 Feature Requirements | PR-56 sampai PR-65 ditambahkan. |
| 11 UX Requirements | Status approval/rejection dan payment gateway diperjelas. |
| 12 Product States | Booking lifecycle dan payment lifecycle dipisahkan. |
| 10 Product Navigation | Owner payment actions dan admin review/transaction monitoring ditambahkan. |
| 13 Product Analytics | Event approval/rejection dan payment gateway ditambahkan. |
| 14 Success Metrics | Observasi approval dan transaksi ditambahkan; target baseline tidak diubah. |
| 15 MVP Priorities | Approval/rejection dan gateway payment ditegaskan sebagai Must Have. |
| 16 Release Plan | Booking approval masuk Release 2; gateway payment masuk Release 4. |
| 17 Dependencies | Provider gateway, merchant account, dan verifikasi status ditambahkan. |
| 18 Acceptance of MVP | Acceptance criteria approval admin dan gateway payment ditambahkan. |

## Requirement ID

- Requirement baseline PR-01 sampai PR-55 dipertahankan.
- PR-13, PR-15, PR-43, PR-44, dan PR-45 diperjelas melalui addendum.
- PR-56 sampai PR-65 merupakan requirement tambahan.

## Keputusan yang masih terbuka

- Provider dan metode pembayaran gateway.
- Pembayaran tunai/manual sebagai alternatif.
- SLA approval dan aturan slot hold/expiry.
- Retry, rekonsiliasi, refund, deposit, dan pembayaran parsial.
- Booking yang dibuat admin atas nama pemilik.
- Pemetaan status invoice dan payment.

## Preservasi baseline

PDF PRD v1.0 tetap dipertahankan dan tidak diedit. Addendum ini mencatat refinement secara terpisah agar riwayat dokumen dan requirement tetap dapat ditelusuri.

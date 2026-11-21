# RabbitCare - MRD Change Log v1.1

**Tanggal awal:** 20 November 2026  
**Pembaruan terakhir:** 21 November 2026  
**Baseline:** Market Requirements Document v1.0 (Draft)  
**Status:** Proposed refinement / Draft - pending stakeholder validation

## Keputusan Arah Produk

| ID | Area | Keputusan | Konsekuensi pada MRD |
|---|---|---|---|
| DEC-01 | Booking | Owner-submitted booking requires admin approval. | Booking yang diajukan pemilik menjadi permintaan yang menunggu approval/rejection admin sebelum terkonfirmasi. Booking yang dibuat admin atas nama pemilik memerlukan aturan terpisah dan validasi stakeholder. |
| DEC-02 | Payment | Payment gateway termasuk MVP. | Pembayaran invoice melalui gateway, verifikasi status transaksi, pemantauan transaksi, dan bukti pembayaran digital menjadi kebutuhan MVP. |

## Perubahan yang Dicatat

| Bagian Baseline | Perubahan v1.1 |
|---|---|
| 10. Persona | Kebutuhan pemilik dan admin diperjelas untuk approval booking dan transaksi gateway. |
| 11. Jobs to Be Done | Ditambahkan kebutuhan kepastian booking dan pembayaran digital. |
| 12. Value Proposition | Nilai pengelolaan booking yang lebih terstruktur dan pembayaran digital melalui gateway ditegaskan. |
| 14.1 Owner/Admin Modules | Status approval booking, gateway payment, status transaksi, dan payment receipt ditambahkan. |
| 14.2 Out of Scope | Integrasi payment gateway dikeluarkan dari daftar out-of-scope karena masuk MVP. |
| 15. Market Requirements | MR-21 sampai MR-23 ditambahkan; MR-04 dan MR-15 diperjelas. |
| 16. MoSCoW | Integrasi gateway dan bukti transaksi berhasil ditetapkan sebagai Must Have. |
| 17.2 Booking Journey | Ditambahkan status menunggu konfirmasi, approval/rejection admin, dan notifikasi hasil keputusan. |
| 17.5 Payment Journey | Alur pembayaran diperbarui menggunakan gateway dan verifikasi hasil transaksi oleh sistem. |
| 20. User Access Rights | Hak Owner untuk memulai pembayaran gateway dan hak Admin untuk memantau transaksi diperjelas. |
| 22. Success Metrics | Indikator booking approval dan status transaksi ditambahkan. Bukti transaksi gateway berhasil diperjelas tanpa menetapkan target numerik baru. |
| 27. Product Launch | Integrasi payment gateway dipindahkan ke MVP. |
| 28. Initial Market Acceptance Criteria | Ditambahkan kriteria penerimaan untuk booking approval admin dan verifikasi transaksi gateway. |
| 29. Market Validation | Pertanyaan validasi diperluas untuk provider, metode pembayaran, biaya, alternatif tunai/manual, dan penanganan transaksi. |

## Penambahan dan Klarifikasi dalam Addendum v1.1

| Bagian Addendum | Perubahan |
|---|---|
| 4.4 User Access Rights Clarification | Menjelaskan hak tambahan Owner dan Admin serta mempertahankan hak akses role lainnya sesuai MRD v1.0. |
| 8.1 Market Acceptance Criteria | Menambahkan tujuh kriteria penerimaan terkait booking approval dan pembayaran gateway. |
| 12. Traceability | Menambahkan keterlacakan antara kebutuhan pasar, BRD, PRD, dan SRD. |
| 11. Open Decisions | Memperjelas keputusan operasional dan pembayaran yang masih memerlukan validasi stakeholder. |

## Catatan Validasi

Keputusan dan ketentuan berikut masih memerlukan validasi stakeholder:

- Penguncian slot selama menunggu approval, termasuk batas waktu dan expiry.
- SLA admin untuk meninjau dan menyetujui atau menolak booking.
- Aturan booking yang dibuat admin atas nama pemilik.
- Provider payment gateway, metode pembayaran, biaya, dan proses settlement.
- Pembayaran tunai/manual sebagai alternatif.
- Refund, deposit, dan pembayaran parsial.
- Penanganan transaksi pending, gagal, atau notifikasi provider yang terlambat.
- Kebijakan payment retry dan rekonsiliasi transaksi.

## Catatan Preservasi Baseline

PDF MRD v1.0 tetap dipertahankan tanpa perubahan. Addendum v1.1 mencatat refinement pada bagian yang terdampak dan tidak menggantikan ketentuan lain yang tidak disebutkan.

Dokumen ini merupakan catatan perubahan terhadap baseline, bukan bukti bahwa seluruh keputusan telah disetujui atau divalidasi oleh pihak klinik.
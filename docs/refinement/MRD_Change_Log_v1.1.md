# RabbitCare - MRD Change Log v1.1

**Tanggal:** 20 November 2026  
**Baseline:** Market Requirements Document v1.0 (Draft)  
**Status:** Proposed refinement / Draft - pending stakeholder validation

## Keputusan arah produk

| ID | Area | Keputusan | Konsekuensi pada MRD |
|---|---|---|---|
| DEC-01 | Booking | Booking memerlukan persetujuan admin. | Booking mandiri menjadi pengajuan yang menunggu approval/rejection admin. |
| DEC-02 | Payment | Payment gateway termasuk MVP. | Pembayaran digital melalui gateway dan status/bukti transaksi menjadi kebutuhan MVP. |

## Perubahan yang dicatat

| Bagian baseline | Perubahan v1.1 |
|---|---|
| 10 Persona | Kebutuhan pemilik dan admin diperjelas untuk approval booking dan transaksi gateway. |
| 11 Jobs to Be Done | Ditambahkan kebutuhan kepastian booking dan pembayaran digital. |
| 12 Value Proposition | Nilai pengelolaan booking dan pembayaran digital ditegaskan. |
| 14.1 Owner/Admin Modules | Status approval booking, gateway payment, status transaksi, dan receipt ditambahkan. |
| 14.2 Out of Scope | Integrasi payment gateway dikeluarkan dari daftar out-of-scope karena masuk MVP. |
| 15 Market Requirements | MR-21 sampai MR-23 ditambahkan; MR-04 dan MR-15 diperjelas. |
| 16 MoSCoW | Gateway payment dan bukti transaksi berhasil dipindahkan/ditetapkan sebagai Must Have. |
| 17 User Journeys | Alur booking dan pembayaran diperbarui. |
| 22 Success Metrics | Pengukuran booking approval dan status transaksi ditambahkan tanpa target numerik baru. |
| 27 Launch Strategy | Gateway payment dipindahkan ke MVP. |
| 29 Market Validation | Pertanyaan validasi diperbarui untuk provider, metode, biaya, dan penanganan transaksi. |

## Catatan validasi

- Penguncian slot selama menunggu approval perlu divalidasi, termasuk expiry.
- SLA approval admin belum ditetapkan.
- Provider dan metode gateway belum dipilih.
- Pembayaran tunai/manual sebagai alternatif belum diputuskan.
- Refund, deposit, pembayaran parsial, retry, dan rekonsiliasi memerlukan keputusan terpisah.

## Catatan preservasi baseline

PDF MRD v1.0 tidak diubah. Addendum v1.1 mencatat bagian yang terdampak dan tidak menggantikan ketentuan lain yang tidak disebutkan.

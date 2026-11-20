# RabbitCare - BRD Change Log v1.1

**Tanggal:** 20 November 2026  
**Baseline:** Business Requirements Document v1.0 (Draft)  
**Status:** Proposed refinement / Draft - pending stakeholder validation

## Keputusan arah produk

| ID | Area | Keputusan | Konsekuensi pada BRD |
|---|---|---|---|
| DEC-01 | Booking | Booking memerlukan persetujuan admin. | Booking yang diajukan pemilik menunggu keputusan admin sebelum terkonfirmasi. |
| DEC-02 | Payment | Payment gateway termasuk dalam MVP. | Integrasi gateway menjadi bagian ruang lingkup awal dan status pembayaran gateway harus dapat ditelusuri. |

## Perubahan yang dicatat

| Bagian baseline | Perubahan v1.1 |
|---|---|
| 6.2 Admin | Peran admin diperbarui untuk review/approval booking dan pemantauan transaksi gateway. |
| 6.6 Penyedia eksternal | Payment gateway menjadi dependensi MVP; provider belum dipilih. |
| 8 Kondisi Target | Tahap review admin ditambahkan pada alur booking. |
| 9 Ruang Lingkup | Integrasi payment gateway ditegaskan termasuk MVP. |
| 10 Business Requirements | BR-53 sampai BR-56 ditambahkan. |
| 11 Business Rules | BRule-10 diperjelas; BRule-15 dan BRule-16 ditambahkan. |
| 12 Business Process | Alur booking dan pembayaran diperbarui. |
| 13 Asumsi | Asumsi pembayaran manual tanpa gateway perlu direvisi; opsi tunai/manual masih terbuka. |
| 14-16 | Risiko dan kriteria penerimaan terkait approval dan gateway ditambahkan. |

## Proposed controls yang masih perlu validasi

- Slot booking yang menunggu persetujuan diusulkan tetap ditahan agar tidak terjadi double booking.
- Slot yang ditolak atau dibatalkan diusulkan dilepaskan.
- SLA review dan expiry slot belum ditetapkan.
- Mekanisme rekonsiliasi transaksi gateway belum ditentukan.

## Keputusan yang masih terbuka

1. Provider dan metode pembayaran gateway.
2. Apakah pembayaran tunai/manual tetap tersedia.
3. SLA persetujuan booking dan expiry slot.
4. Perlakuan booking yang dibuat admin atas nama pemilik.
5. Refund, pembatalan transaksi, dan rekonsiliasi.
6. Pembayaran setelah layanan atau penggunaan deposit.

## Catatan preservasi baseline

PDF BRD v1.0 tidak diubah. Addendum v1.1 hanya mencatat bagian yang terdampak dan tidak menggantikan ketentuan lain yang tidak disebutkan.

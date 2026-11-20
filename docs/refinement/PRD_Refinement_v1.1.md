# RabbitCare - PRD Refinement Addendum v1.1

**Tanggal refinement:** 21 November 2026  
**Dokumen sumber:** RabbitCare Product Requirements Document (PRD) v1.0  
**Baseline:** MVP 1.0 / Draft  
**Status addendum:** Proposed refinement / Draft - pending stakeholder validation  
**Jenis perubahan:** Addendum terhadap baseline; bukan penggantian dokumen asli

## Ringkasan keputusan produk

Addendum ini menerjemahkan dua keputusan produk yang telah disepakati ke dalam kebutuhan dan perilaku produk:

1. **Booking memerlukan keputusan admin.** Pemilik tetap dapat mengajukan booking melalui Owner Portal. Pengajuan tidak langsung menjadi booking terkonfirmasi; admin klinik harus menyetujui atau menolak permintaan tersebut.
2. **Payment gateway termasuk dalam MVP.** Pemilik membayar invoice melalui payment gateway yang dikonfigurasi. Status pembayaran dan bukti transaksi hanya diperbarui/diterbitkan setelah hasil transaksi dari provider diverifikasi sistem.

PRD v1.0 tetap dipertahankan sebagai baseline. Addendum ini hanya memperbarui bagian yang terdampak dan menambahkan requirement PR-56 sampai PR-65. Requirement PR-01 sampai PR-55 tetap berlaku, kecuali bagian yang secara eksplisit diperjelas di sini.

## 1. Tujuan dan cara membaca addendum

Tujuan refinement adalah memastikan product scope, user journey, feature requirements, product states, analytics, MVP priorities, release plan, dependencies, dan acceptance criteria konsisten dengan keputusan booking admin approval dan payment gateway pada MVP.

Gunakan PRD v1.0 sebagai sumber baseline. Jika bagian dalam addendum ini memperjelas atau mengubah perilaku produk, rumusan addendum menjadi usulan terbaru untuk validasi stakeholder. Bagian lain yang tidak disebutkan tetap mengikuti PRD v1.0.

## 2. Ringkasan perubahan terhadap PRD v1.0

| Bagian PRD v1.0 | Baseline | Refinement v1.1 |
|---|---|---|
| 4 Product Goals | PG-02: booking dapat dilakukan secara mandiri. | Diperjelas: pemilik mengajukan booking dan admin mengambil keputusan approval/rejection. |
| 6 Product Users | Admin mengelola booking, tagihan, dan pembayaran. | Admin memiliki review queue booking serta pemantauan transaksi gateway dan penanganan pengecualian. |
| 7 Product Scope | Owner booking; Clinic Portal mengelola booking, billing, dan payment. | Owner melihat status keputusan; admin approve/reject; owner membayar invoice via gateway; admin memantau transaksi. |
| 8.2 Booking Journey | Booking dibuat dan konfirmasi dikirim. | Pengajuan menjadi `MENUNGGU_KONFIRMASI`, lalu admin approve atau reject. |
| 8.5 Payment Journey | Metode dipilih, pembayaran diproses, status diperbarui, bukti diterbitkan. | Pembayaran menggunakan gateway; hasil provider diverifikasi sebelum status PAID dan receipt diterbitkan. |
| 9.3 PR-13 dan PR-15 | Pemilik membuat booking; admin mengonfirmasi booking. | Diperjelas sebagai request dan keputusan admin; acceptance criteria rejection ditambahkan. |
| 9.8 PR-43 sampai PR-45 | Admin mencatat pembayaran; receipt diterbitkan; booking berubah menjadi lunas. | Diselaraskan dengan gateway; receipt mengikuti transaksi terverifikasi; payment state dipisahkan dari booking lifecycle. |
| 11 UX Requirements | Label status booking konsisten; pesan error menyebut penyebab dan tindakan. | Ditambahkan tampilan status menunggu keputusan/ditolak dan status transaksi gateway. |
| 12 Product States | Daftar status booking juga memuat `BELUM_DIBAYAR` dan `LUNAS`; payment punya status sendiri. | Status booking dan payment dipisahkan agar tidak mencampur dua lifecycle. |
| 13 Product Analytics | `booking_created`, `payment_completed`, dan event lain. | Ditambahkan event approval/rejection dan inisiasi/hasil transaksi gateway. |
| 14 Success Metrics | Target awal booking, layanan, dan bukti pembayaran. | Target asli dipertahankan; ditambahkan pengukuran waktu approval dan hasil transaksi. |
| 15 MVP Priorities | Booking dan pembayaran tercantum Must Have. | Admin approval/rejection dan gateway invoice payment ditegaskan sebagai Must Have. |
| 16 Release Plan | Release 2 Scheduling and Booking; Release 4 Billing and Reminder. | Approval/rejection dimasukkan ke Release 2; gateway payment ke Release 4. |
| 17 Dependencies | Backend, database, notification provider, hosting, data klinik. | Ditambahkan provider gateway, merchant account, metode pembayaran, dan mekanisme verifikasi status. |
| 18 Acceptance of MVP | Booking dapat dibuat; tagihan dan pembayaran dapat dicatat. | MVP harus mendukung keputusan admin dan pembayaran gateway terverifikasi serta receipt. |

## 3. Product goals dan pengguna

### 3.1 Penyesuaian Product Goal PG-02

**Baseline PG-02:** Memungkinkan booking dilakukan secara mandiri.

**Rumusan refinement:** Memungkinkan pemilik mengajukan booking secara mandiri dan memperoleh kepastian jadwal setelah admin klinik menyetujui atau menolak pengajuan tersebut.

Tujuan lain PG-01 dan PG-03 sampai PG-08 tetap mengikuti PRD v1.0.

### 3.2 Pemilik kelinci - Owner Portal

Tambahan perilaku produk:
- Pemilik dapat mengirim pengajuan booking.
- Pemilik dapat melihat bahwa pengajuan sedang menunggu keputusan admin.
- Pemilik dapat melihat hasil persetujuan atau penolakan.
- Jika ditolak, pemilik dapat melihat alasan penolakan apabila alasan tersebut dicatat admin.
- Pemilik dapat membuka invoice dan memulai pembayaran melalui payment gateway.
- Pemilik dapat melihat status transaksi dan bukti pembayaran setelah transaksi berhasil diverifikasi.

### 3.3 Admin - Clinic Portal

Tambahan perilaku produk:
- Admin dapat membuka daftar pengajuan booking yang menunggu keputusan.
- Admin dapat menyetujui atau menolak pengajuan.
- Admin wajib mencatat alasan ketika menolak booking.
- Sistem mencatat admin yang mengambil keputusan dan waktu keputusan.
- Admin dapat memantau status transaksi gateway, referensi transaksi, dan transaksi yang membutuhkan tindak lanjut.
- Admin tidak menandai transaksi gateway sebagai berhasil hanya berdasarkan redirect browser atau klaim/bukti yang dikirim pemilik.

Kebutuhan dokter, groomer, dan manajer tetap mengikuti PRD v1.0.

## 4. Product scope yang diperbarui

### 4.1 Owner Portal - termasuk MVP

Seluruh fitur baseline tetap berlaku, dengan penegasan berikut:
- Booking layanan menghasilkan pengajuan dengan status awal `MENUNGGU_KONFIRMASI`.
- Halaman booking menampilkan status pengajuan, persetujuan, atau penolakan.
- Pemilik dapat melihat alasan penolakan jika tersedia.
- Halaman tagihan menyediakan tindakan pembayaran melalui gateway yang dikonfigurasi.
- Pemilik dapat melihat status transaksi dan bukti pembayaran untuk transaksi yang berhasil.

### 4.2 Clinic Portal - termasuk MVP

Seluruh fitur baseline tetap berlaku, dengan penegasan berikut:
- Modul Booking menyediakan review queue untuk admin.
- Admin dapat approve atau reject pengajuan booking.
- Keputusan dan alasan penolakan tersimpan pada riwayat booking.
- Modul Billing/Payment menyediakan pemantauan status transaksi gateway.
- Admin dapat mengidentifikasi transaksi pending, failed, atau membutuhkan tindak lanjut.

### 4.3 Batasan dan hal yang belum diputuskan

Addendum ini tidak memilih provider gateway tertentu dan tidak menetapkan metode pembayaran, biaya provider, settlement, atau SLA approval. Hal-hal berikut masih memerlukan keputusan terpisah:
- Provider gateway dan metode pembayaran yang akan diaktifkan.
- Apakah pembayaran tunai/manual tersedia sebagai alternatif gateway.
- Booking yang dibuat admin atas nama pemilik: apakah juga memerlukan approval.
- SLA admin untuk memproses pengajuan booking.
- Berapa lama slot ditahan saat booking menunggu keputusan dan kapan slot kedaluwarsa.
- Deposit, pembayaran parsial, refund otomatis, dan multi-provider.
- Aturan retry pembayaran dan prosedur rekonsiliasi transaksi.

## 5. Core user journeys yang diperbarui

### 5.1 Booking layanan - owner request dan admin decision

1. Pemilik memilih profil kelinci.
2. Pemilik memilih jenis layanan.
3. Sistem menampilkan slot yang tersedia sesuai jadwal klinik.
4. Pemilik memilih tanggal dan slot.
5. Sistem memvalidasi ulang ketersediaan slot.
6. Pemilik mengirim pengajuan booking.
7. Sistem membuat kode booking dan menyimpan status `MENUNGGU_KONFIRMASI`.
8. Sistem menampilkan bahwa pengajuan telah diterima dan mengirim pemberitahuan sesuai kanal yang tersedia.
9. Admin membuka review queue dan memeriksa detail permintaan.
10. Admin menyetujui atau menolak pengajuan.
11. Jika disetujui, status booking menjadi `TERKONFIRMASI`; pemilik menerima notifikasi.
12. Jika ditolak, status booking menjadi `DITOLAK`; alasan penolakan disimpan dan pemilik menerima notifikasi.

**Aturan slot masih terbuka:** Sistem harus mencegah dua booking terkonfirmasi pada slot yang sama. Kebijakan apakah permintaan `MENUNGGU_KONFIRMASI` langsung menahan slot, batas waktu penahanan, dan pelepasan slot setelah rejection/expiry harus ditetapkan bersama stakeholder sebelum implementasi final.

### 5.2 Pembayaran invoice melalui payment gateway

1. Admin membuka tagihan yang berasal dari layanan sesuai proses baseline.
2. Sistem menampilkan rincian invoice kepada pemilik.
3. Pemilik memilih tindakan untuk membayar invoice.
4. Sistem memulai transaksi melalui payment gateway yang telah dikonfigurasi.
5. Sistem menyimpan referensi pembayaran yang menghubungkan percobaan transaksi dengan invoice.
6. Provider memproses transaksi dan menyampaikan status transaksi.
7. Sistem memverifikasi hasil transaksi melalui mekanisme provider yang ditetapkan pada desain teknis.
8. Jika hasil terverifikasi berhasil, status pembayaran menjadi `PAID`, invoice ditandai lunas, dan bukti pembayaran diterbitkan.
9. Jika status masih pending atau gagal, sistem menampilkan status yang sesuai dan tidak menerbitkan bukti pembayaran lunas.
10. Admin memantau transaksi dan menindaklanjuti pengecualian.

Redirect kembali ke aplikasi tidak boleh menjadi satu-satunya dasar untuk menetapkan transaksi berhasil. Detail callback, signature verification, idempotency, dan rekonsiliasi dijabarkan pada SRD.

## 6. Feature requirements yang diperbarui

### 6.1 Penyesuaian requirement yang sudah ada

| ID | Rumusan baseline | Rumusan refinement / acceptance criteria |
|---|---|---|
| PR-13 | Pemilik dapat membuat booking. | Pemilik dapat mengirim pengajuan booking. Sistem menghasilkan kode unik dan status awal `MENUNGGU_KONFIRMASI`; pengajuan belum dianggap terkonfirmasi. |
| PR-15 | Admin dapat mengonfirmasi booking. | Admin dapat menyetujui atau menolak pengajuan. Approval mengubah status menjadi `TERKONFIRMASI`; rejection mengubah status menjadi `DITOLAK` dan mensyaratkan alasan penolakan. |
| PR-43 | Admin dapat mencatat pembayaran. | Admin dapat memantau status pembayaran gateway dan referensi transaksi. Penetapan pembayaran sukses mengikuti hasil provider yang telah diverifikasi sistem. Pencatatan pembayaran manual belum ditetapkan sebagai jalur MVP. |
| PR-44 | Sistem menerbitkan bukti pembayaran. | Sistem menerbitkan bukti pembayaran setelah status transaksi `PAID` terverifikasi. Bukti memiliki nomor/referensi transaksi unik dan terhubung ke invoice. |
| PR-45 | Booking berubah menjadi lunas. | Status pembayaran/invoice diperbarui menjadi lunas setelah transaksi `PAID` terverifikasi. Status lifecycle booking tidak diubah menjadi status pembayaran. |

### 6.2 Requirement tambahan

PR-56 sampai PR-65 ditambahkan setelah requirement terakhir pada PRD v1.0 (PR-55).

| ID | Product Requirement | Acceptance Criteria |
|---|---|---|
| PR-56 | Admin dapat meninjau pengajuan booking dan mengambil keputusan approve atau reject. | Daftar pengajuan menunggu tampil bagi admin berwenang; keputusan tersimpan dan status berubah sesuai hasil. |
| PR-57 | Sistem menyimpan detail keputusan booking. | Waktu keputusan dan identitas admin tersimpan; alasan wajib diisi saat reject. |
| PR-58 | Pemilik dapat melihat status dan hasil keputusan booking. | Owner Portal menampilkan `MENUNGGU_KONFIRMASI`, `TERKONFIRMASI`, atau `DITOLAK`; alasan penolakan ditampilkan jika dicatat. |
| PR-59 | Sistem mencegah lebih dari satu booking terkonfirmasi pada slot yang sama. | Dua permintaan tidak dapat sama-sama disetujui pada slot yang sama; aturan hold/expiry untuk status menunggu mengikuti kebijakan klinik yang ditetapkan. |
| PR-60 | Pemilik dapat memulai pembayaran invoice melalui payment gateway yang dikonfigurasi. | Sistem membuat alur pembayaran untuk invoice yang dapat dibayar dan menghubungkan transaksi dengan invoice yang benar. |
| PR-61 | Sistem menyimpan referensi unik untuk setiap percobaan pembayaran gateway. | Referensi transaksi dapat ditelusuri ke invoice dan percobaan pembayaran terkait; nilai invoice yang dibayar dapat diverifikasi. |
| PR-62 | Sistem memperbarui status pembayaran berdasarkan hasil gateway yang diverifikasi. | Status pending/failed tidak ditampilkan sebagai paid; redirect browser saja tidak menetapkan pembayaran berhasil. |
| PR-63 | Sistem menerbitkan bukti pembayaran untuk transaksi gateway yang berhasil. | Bukti hanya diterbitkan setelah status `PAID` terverifikasi dan memuat referensi transaksi serta rincian invoice. |
| PR-64 | Admin dapat memantau status transaksi gateway dan transaksi yang membutuhkan tindak lanjut. | Admin dapat melihat status, referensi transaksi, invoice terkait, dan indikasi transaksi pending/failed. |
| PR-65 | Pemrosesan hasil transaksi gateway harus mencegah duplikasi. | Pemberitahuan/status berulang untuk transaksi yang sama tidak membuat pembayaran atau bukti pembayaran ganda. |

## 7. Product states yang diperbarui

PRD v1.0 mencantumkan status `BELUM_DIBAYAR` dan `LUNAS` di dalam daftar status booking, sekaligus memiliki daftar status pembayaran terpisah. Untuk menghindari pencampuran dua lifecycle, addendum mengusulkan pemisahan berikut.

### 7.1 Booking lifecycle

| Status | Makna |
|---|---|
| `MENUNGGU_KONFIRMASI` | Pengajuan diterima dan menunggu keputusan admin. |
| `TERKONFIRMASI` | Admin menyetujui booking. |
| `DITOLAK` | Admin menolak pengajuan booking. |
| `CHECK_IN` | Pemilik/pasien telah check-in. |
| `MENUNGGU_ANTREAN` | Pasien menunggu giliran layanan. |
| `SEDANG_DILAYANI` | Layanan sedang berlangsung. |
| `SELESAI_DILAYANI` | Layanan selesai. |
| `DIBATALKAN` | Booking dibatalkan sesuai aturan. |
| `DIJADWALKAN_ULANG` | Booking dijadwalkan ulang sesuai aturan. |
| `TIDAK_HADIR` | Pemilik tidak hadir sesuai jadwal. |

### 7.2 Payment lifecycle

Status pembayaran pada PRD v1.0 tetap dipertahankan:
- `PENDING`
- `PAID`
- `FAILED`
- `CANCELLED`
- `REFUNDED`

`REFUNDED` dipertahankan sebagai status yang telah tercantum pada baseline, tetapi kemampuan refund dan otomasi refund belum otomatis menjadi bagian MVP. Proses refund perlu keputusan dan requirement tersendiri.

`BELUM_DIBAYAR` dan `LUNAS` sebaiknya menjadi ringkasan status invoice, bukan status lifecycle booking. Pemetaan status invoice dan pembayaran secara rinci perlu dijelaskan pada SRD.

## 8. UX requirements yang diperbarui

Tambahan pada UX Requirements bagian 11:
- Owner Portal harus membedakan pengajuan booking yang menunggu keputusan, disetujui, dan ditolak.
- Clinic Portal harus menyediakan daftar pengajuan booking yang menunggu review admin.
- Saat booking ditolak, pemilik harus mendapat pesan status dan alasan penolakan jika tersedia.
- Owner Portal harus menampilkan status transaksi gateway yang jelas: pending, paid, failed, atau cancelled.
- Sistem tidak boleh menampilkan status paid atau bukti lunas sebelum hasil transaksi diverifikasi.
- Saat transaksi masih pending, tindakan pembayaran tidak boleh menghasilkan pembayaran ganda untuk transaksi yang sama.
- Pesan error pembayaran harus menjelaskan kondisi dan tindakan berikutnya tanpa mengungkap data sensitif provider.

UX requirements lain pada PRD v1.0 tetap berlaku.

## 9. Product navigation yang diperbarui

### Owner Portal
- Dashboard
- Profil Kelinci
- Booking Layanan
  - Pengajuan Booking
  - Status dan Hasil Keputusan
- Kalender
- Riwayat Kesehatan
- Riwayat Grooming
- Vaksinasi
- Tagihan
  - Pembayaran via Gateway
  - Status Transaksi
  - Bukti Pembayaran
- Reminder
- Profil Akun

### Clinic Portal
- Dashboard
- Data Pemilik
- Data Pasien
- Jadwal
- Booking
  - Review Queue
  - Approval / Rejection
- Check-in
- Antrean
- Pemeriksaan
- Vaksinasi
- Grooming
- Tagihan
- Pembayaran
  - Monitoring Transaksi Gateway
  - Penanganan Pengecualian
- Reminder
- Laporan
- Pengaturan

Susunan ini merupakan penjelasan navigasi produk yang diusulkan; label final dapat disesuaikan saat desain UI.

## 10. Product analytics dan success metrics

### 10.1 Event analytics tambahan

Event pada PRD v1.0 tetap dipertahankan. Tambahkan:
- `booking_submitted`
- `booking_approved`
- `booking_rejected`
- `payment_initiated`
- `payment_status_updated`
- `payment_failed`
- `payment_completed` (dipertahankan; hanya dicatat untuk transaksi berhasil yang terverifikasi)

### 10.2 Success metrics tambahan

Target numerik pada PRD v1.0 tidak diubah. Tambahkan metrik observasi:
- Waktu dari `booking_submitted` sampai keputusan admin.
- Jumlah dan proporsi booking approved/rejected.
- Jumlah booking yang masih menunggu keputusan.
- Jumlah transaksi gateway pending, paid, failed, atau cancelled.
- Proporsi transaksi paid yang memiliki bukti pembayaran dan referensi transaksi.

Target baru belum ditetapkan karena perlu data pilot. Target awal pada PRD v1.0 tetap merupakan target awal, bukan hasil pengukuran aktual.

## 11. Prioritas MVP dan release plan

### 11.1 Prioritas MVP

Tambahan/penegasan pada kategori **Must Have**:
- Owner dapat mengajukan booking.
- Admin dapat approve/reject booking.
- Owner dapat melihat hasil keputusan booking.
- Payment gateway untuk pembayaran invoice.
- Status transaksi gateway yang diverifikasi.
- Bukti pembayaran setelah transaksi berhasil.
- Admin dapat memantau status transaksi dan pengecualian.

Fitur Should Have dan Could Have lain tetap mengikuti PRD v1.0. Reschedule dan pembatalan tetap mengikuti prioritas baseline dan tidak otomatis berubah menjadi Must Have.

### 11.2 Release plan

| Release | Baseline PRD v1.0 | Refinement v1.1 |
|---|---|---|
| Release 1: Foundation | Authentication, role, profil pemilik/kelinci, data petugas. | Tidak ada perubahan. |
| Release 2: Scheduling and Booking | Jadwal, booking, konfirmasi, pembatalan, check-in, antrean. | Tambahkan admin review queue, approval/rejection, status keputusan, dan notifikasi hasil. |
| Release 3: Service Records | Rekam medis, vaksinasi, grooming, rujukan, kontrol. | Tidak ada perubahan. |
| Release 4: Billing and Reminder | Tagihan, pembayaran, bukti pembayaran, reminder, notifikasi. | Pembayaran invoice melalui gateway, verifikasi status transaksi, monitoring admin, dan receipt setelah transaksi sukses. |
| Release 5: Reporting | Dashboard, laporan, audit log, export data. | Tambahkan event/metrik approval booking dan transaksi gateway sesuai kebutuhan analitik. |

## 12. Dependencies dan risiko produk

### 12.1 Dependencies tambahan
- Provider payment gateway telah dipilih dan menyediakan dokumentasi integrasi.
- Klinik memiliki merchant account dan kredensial yang diperlukan.
- Metode pembayaran yang dibutuhkan tersedia pada provider.
- Sistem dapat menerima dan memverifikasi status transaksi dari provider melalui mekanisme yang ditetapkan pada SRD.
- Admin klinik memiliki kapasitas untuk meninjau booking.
- Aturan hold/expiry slot telah disepakati oleh klinik.
- Notification provider dapat menyampaikan notifikasi pengajuan dan hasil keputusan booking.

### 12.2 Risiko dan mitigasi yang perlu direncanakan

| Risiko | Dampak produk | Mitigasi/keputusan yang diperlukan |
|---|---|---|
| Admin terlambat memutuskan booking | Pemilik tidak memperoleh kepastian jadwal. | Tentukan SLA review dan eskalasi. |
| Slot ditahan terlalu lama | Slot tidak tersedia bagi pemilik lain. | Tentukan hold duration dan expiry. |
| Status gateway terlambat | Status transaksi di aplikasi belum mutakhir. | Tentukan monitoring dan rekonsiliasi. |
| Transaksi gagal atau pending | Pemilik belum dapat menyelesaikan pembayaran. | Rancang pesan status dan kebijakan retry. |
| Hasil transaksi duplikat | Risiko pembayaran/receipt tercatat ganda. | Pastikan pemrosesan hasil transaksi idempotent. |

## 13. Acceptance of MVP yang diperbarui

MVP dapat diterima apabila seluruh acceptance criteria PRD v1.0 yang tidak berubah tetap terpenuhi, serta:
1. Pemilik dapat mengirim pengajuan booking dan menerima status `MENUNGGU_KONFIRMASI`.
2. Admin berwenang dapat approve atau reject pengajuan booking.
3. Sistem mencatat identitas admin, waktu keputusan, dan alasan rejection.
4. Pemilik dapat melihat hasil keputusan booking.
5. Sistem mencegah dua booking terkonfirmasi pada slot yang sama.
6. Pemilik dapat memulai pembayaran invoice melalui payment gateway yang dikonfigurasi.
7. Status pembayaran hanya menjadi `PAID` setelah hasil provider diverifikasi.
8. Bukti pembayaran diterbitkan setelah transaksi berhasil diverifikasi.
9. Admin dapat memantau status transaksi dan mengidentifikasi transaksi yang perlu tindak lanjut.
10. Hasil transaksi gateway berulang tidak menghasilkan pencatatan pembayaran atau bukti ganda.
11. Status booking dan status pembayaran ditampilkan sebagai lifecycle yang berbeda.

## 14. Keputusan terbuka sebelum sign-off

- Provider gateway, metode pembayaran, biaya, dan settlement.
- Apakah pembayaran tunai/manual tersedia sebagai alternatif.
- Kebijakan booking yang dibuat admin atas nama pemilik.
- SLA approval admin.
- Penahanan slot, expiry, dan pelepasan slot.
- Retry pembayaran, rekonsiliasi, refund, deposit, dan pembayaran parsial.
- Pemetaan rinci status invoice terhadap status payment gateway.
- Kanal notifikasi dan template pesan untuk approval/rejection.

## 15. Keterlacakan dan status dokumen

| Area | Dokumen lanjutan |
|---|---|
| Booking approval/rejection | BRD business rules, SRD booking workflow, dan activity/sequence diagram booking. |
| Slot hold/expiry | BRD booking rules dan SRD scheduling/booking logic. |
| Gateway payment | BRD billing requirements, SRD payment integration, dan payment sequence/activity diagram. |
| Payment status dan receipt | SRD transaction/data requirements. |
| Product analytics | Implementasi event tracking dan dashboard reporting. |

**Status akhir:** Proposed refinement / Draft - pending stakeholder validation. PRD v1.0 tetap menjadi baseline dan tidak diubah. Addendum ini bukan bukti bahwa provider gateway, aturan operasional, atau target produk telah disetujui stakeholder.

# RabbitCare - PRD Refinement Addendum v1.1

**Tanggal refinement:** 21 November 2026  
**Dokumen sumber:** RabbitCare Product Requirements Document (PRD) v1.0  
**Baseline:** MVP 1.0 / Draft  
**Status addendum:** Proposed refinement / Draft - pending stakeholder validation  
**Jenis perubahan:** Addendum terhadap baseline; bukan penggantian dokumen asli

## Ringkasan keputusan produk

Addendum ini menerjemahkan dua keputusan produk yang menjadi dasar refinement dan masih menunggu validasi stakeholder:

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
| 9.3 PR-13, PR-14, dan PR-15 | Booking diperjelas sebagai pengajuan dan admin dapat approve/reject; aturan double booking diselaraskan dengan proses approval. |
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
- Jika booking ditolak, pemilik dapat melihat alasan penolakan yang wajib dicatat oleh admin.
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
3. Sistem menampilkan pilihan tanggal dan slot berdasarkan jadwal klinik, ketersediaan petugas, serta kapasitas layanan.
4. Pemilik memilih tanggal dan slot yang tersedia.
5. Sistem memvalidasi ulang ketersediaan jadwal dan kapasitas sebelum pengajuan disimpan.
6. Pemilik mengirim pengajuan booking.
7. Sistem membuat kode booking unik dan menyimpan status awal `MENUNGGU_KONFIRMASI`.
8. Sistem menampilkan konfirmasi bahwa pengajuan telah diterima dan mengirim pemberitahuan sesuai kanal yang tersedia.
9. Admin membuka review queue dan memeriksa detail pengajuan, profil kelinci, jenis layanan, tanggal, slot, jadwal petugas, serta kapasitas layanan.
10. Admin menyetujui atau menolak pengajuan.
11. Sebelum approval disimpan, sistem memvalidasi ulang ketersediaan kapasitas untuk mencegah konflik jadwal dan double booking.
12. Jika kapasitas masih tersedia, status booking berubah menjadi `TERKONFIRMASI` dan kapasitas terkait diperbarui.
13. Jika kapasitas tidak tersedia, sistem menolak proses approval dan menampilkan informasi kepada admin agar pengajuan dapat ditindaklanjuti.
14. Jika admin menolak pengajuan, status booking berubah menjadi `DITOLAK` dan admin wajib mencatat alasan penolakan.
15. Pemilik dapat melihat hasil keputusan dan alasan penolakan melalui Owner Portal serta menerima pemberitahuan sesuai kanal yang tersedia.

**Aturan ketersediaan dan kapasitas slot:**

- Sistem tidak boleh menyetujui booking melebihi kapasitas layanan dan sumber daya yang tersedia pada jadwal terkait.
- Validasi kapasitas dilakukan kembali ketika admin mengambil keputusan approval, bukan hanya ketika pemilik mengirim pengajuan.
- Proses validasi dan penyimpanan approval harus dilakukan secara konsisten agar dua pengajuan tidak menggunakan kapasitas yang sama secara berlebihan.
- Sebagai usulan kebijakan MVP, pengajuan `MENUNGGU_KONFIRMASI` belum mengunci slot secara permanen. Kapasitas baru diperbarui setelah approval berhasil.
- Jika klinik menghendaki slot ditahan sejak pengajuan, durasi penahanan dan aturan expiry harus ditetapkan bersama stakeholder sebelum implementasi.

### 5.2 Pembayaran invoice melalui payment gateway

1. Petugas mencatat layanan dan item yang diberikan kepada kelinci sesuai proses operasional klinik.
2. Sistem membentuk invoice berdasarkan layanan dan item yang telah dicatat.
3. Admin atau petugas berwenang memeriksa dan menerbitkan invoice sesuai prosedur klinik.
4. Sistem menampilkan rincian invoice kepada pemilik melalui Owner Portal.
5. Pemilik membuka invoice dengan status `BELUM_DIBAYAR` dan memilih tindakan pembayaran.
6. Sistem memulai transaksi melalui payment gateway yang telah dikonfigurasi.
7. Sistem membuat dan menyimpan referensi unik untuk setiap percobaan pembayaran serta menghubungkannya dengan invoice terkait.
8. Payment gateway memproses transaksi dan menyampaikan hasil transaksi melalui mekanisme notifikasi server-to-server yang disediakan provider.
9. Sistem memverifikasi keaslian pemberitahuan, referensi transaksi, invoice, nominal pembayaran, dan status transaksi berdasarkan mekanisme provider.
10. Jika transaksi berhasil dan hasilnya telah terverifikasi, status payment attempt berubah menjadi `PAID`, status invoice berubah menjadi `LUNAS`, dan sistem menerbitkan bukti pembayaran.
11. Jika transaksi masih `PENDING`, `FAILED`, atau `CANCELLED`, sistem memperbarui status payment attempt sesuai hasil provider dan invoice tetap `BELUM_DIBAYAR`.
12. Sistem tidak menerbitkan bukti pembayaran lunas untuk transaksi yang belum berhasil diverifikasi.
13. Admin dapat memantau status transaksi, referensi pembayaran, dan transaksi yang membutuhkan tindak lanjut.
14. Jika terdapat lebih dari satu transaksi berhasil untuk invoice yang sama, sistem menandai kondisi tersebut sebagai pengecualian untuk proses rekonsiliasi.

**Aturan pembayaran:**

- Satu invoice menggunakan pembayaran penuh pada MVP.
- Sistem harus mencegah percobaan pembayaran aktif yang tidak perlu untuk invoice yang sama.
- Percobaan ulang dapat dilakukan setelah transaksi sebelumnya dinyatakan gagal atau dibatalkan sesuai status provider.
- Redirect browser tidak boleh menjadi satu-satunya dasar penetapan pembayaran berhasil.
- Pemrosesan notifikasi transaksi harus bersifat idempotent untuk mencegah pencatatan pembayaran dan penerbitan receipt berulang.
- Jika dua transaksi berbeda sama-sama berhasil untuk invoice yang sama, sistem harus menandai transaksi tersebut untuk rekonsiliasi dan tidak menerbitkan bukti pembayaran ganda.

Detail integrasi, signature verification, idempotency, dan rekonsiliasi dijabarkan lebih lanjut pada SRD.

## 6. Feature requirements yang diperbarui

### 6.1 Penyesuaian requirement yang sudah ada

| ID | Rumusan baseline | Rumusan refinement / acceptance criteria |
|---|---|---|
| PR-13 | Pemilik dapat membuat booking. | Pemilik dapat mengirim pengajuan booking. Sistem menghasilkan kode unik dan status awal `MENUNGGU_KONFIRMASI`; pengajuan belum dianggap terkonfirmasi. |
| PR-14 | Sistem mencegah double booking. | Sistem memvalidasi kapasitas layanan dan sumber daya terkait ketika booking diajukan dan ketika admin melakukan approval. Sistem tidak boleh menyetujui booking yang menyebabkan kapasitas terlampaui. |
| PR-15 | Admin dapat mengonfirmasi booking. | Admin dapat menyetujui atau menolak pengajuan setelah memeriksa jadwal dan kapasitas. Approval mengubah status menjadi `TERKONFIRMASI`; rejection mengubah status menjadi `DITOLAK` dan mensyaratkan alasan penolakan. |
| PR-43 | Admin dapat mencatat pembayaran. | Admin dapat memantau status pembayaran gateway dan referensi transaksi. Penetapan pembayaran sukses mengikuti hasil provider yang telah diverifikasi sistem. Pencatatan pembayaran manual belum ditetapkan sebagai jalur MVP. |
| PR-44 | Sistem menerbitkan bukti pembayaran. | Sistem menerbitkan bukti pembayaran setelah status transaksi `PAID` terverifikasi. Bukti memiliki nomor/referensi transaksi unik dan terhubung ke invoice. |
| PR-45 | Booking berubah menjadi lunas. | Status invoice diperbarui menjadi `LUNAS` setelah pembayaran penuh berhasil diverifikasi. Status lifecycle booking tidak diubah menjadi status pembayaran. |


### 6.2 Requirement tambahan

PR-56 sampai PR-65 ditambahkan setelah requirement terakhir pada PRD v1.0 (PR-55).

| ID | Product Requirement | Acceptance Criteria |
|---|---|---|
| PR-56 | Admin dapat meninjau pengajuan booking dan mengambil keputusan approve atau reject. | Daftar pengajuan menunggu tampil bagi admin berwenang. Admin dapat melihat detail pengajuan, jadwal petugas, dan kapasitas layanan sebelum mengambil keputusan. |
| PR-57 | Sistem menyimpan detail keputusan booking. | Identitas admin dan waktu keputusan tersimpan. Alasan wajib diisi ketika admin menolak pengajuan. |
| PR-58 | Pemilik dapat melihat status dan hasil keputusan booking. | Owner Portal menampilkan `MENUNGGU_KONFIRMASI`, `TERKONFIRMASI`, atau `DITOLAK`. Untuk booking yang ditolak, alasan penolakan wajib ditampilkan. |
| PR-59 | Sistem memvalidasi ulang kapasitas ketika admin menyetujui pengajuan booking. | Approval hanya berhasil jika kapasitas layanan dan sumber daya terkait masih tersedia. Sistem mencegah kapasitas terlampaui akibat approval bersamaan. |
| PR-60 | Pemilik dapat memulai pembayaran invoice melalui payment gateway yang dikonfigurasi. | Sistem hanya mengizinkan pembayaran untuk invoice yang masih dapat dibayar. Setiap percobaan pembayaran terhubung dengan invoice yang benar. |
| PR-61 | Sistem menyimpan referensi unik untuk setiap percobaan pembayaran gateway. | Setiap transaksi memiliki referensi unik yang terhubung dengan invoice, nominal pembayaran, mata uang, dan referensi transaksi provider. |
| PR-62 | Sistem memperbarui status pembayaran berdasarkan hasil gateway yang telah diverifikasi. | Status pembayaran diperbarui berdasarkan hasil provider yang diverifikasi sistem. Redirect browser tidak dapat menetapkan status `PAID`. |
| PR-63 | Sistem menerbitkan bukti pembayaran untuk transaksi gateway yang berhasil. | Bukti pembayaran hanya diterbitkan setelah status `PAID` terverifikasi dan memuat referensi transaksi serta rincian invoice. |
| PR-64 | Admin dapat memantau status transaksi gateway dan transaksi yang membutuhkan tindak lanjut. | Admin dapat melihat status transaksi, referensi pembayaran, invoice terkait, serta transaksi pending, failed, atau kondisi pengecualian. |
| PR-65 | Pemrosesan hasil transaksi gateway harus mencegah duplikasi. | Notifikasi berulang untuk transaksi yang sama hanya diproses satu kali. Jika terdapat transaksi berbeda yang sama-sama berhasil untuk invoice yang sama, sistem menandainya untuk rekonsiliasi dan mencegah penerbitan receipt ganda. |

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

### 7.3 Invoice status

Status invoice dipisahkan dari booking lifecycle dan payment transaction lifecycle.

| Status | Makna |
|---|---|
| `BELUM_DIBAYAR` | Invoice belum memiliki pembayaran berhasil yang telah diverifikasi dan memenuhi total tagihan. |
| `LUNAS` | Total invoice telah dipenuhi melalui pembayaran berhasil yang diverifikasi sistem. |

**Aturan perubahan status invoice:**

- Invoice tetap `BELUM_DIBAYAR` ketika payment attempt berstatus `PENDING`, `FAILED`, atau `CANCELLED`.
- Invoice berubah menjadi `LUNAS` setelah sistem memverifikasi pembayaran penuh sesuai total invoice.
- Perubahan status payment attempt tidak secara otomatis mengubah booking lifecycle.
- Status `REFUNDED` pada payment transaction tidak secara otomatis mengubah invoice menjadi `BELUM_DIBAYAR`. Dampak refund terhadap saldo dan status invoice memerlukan keputusan serta requirement tersendiri.

Dalam refinement MVP ini, pembayaran mengasumsikan pembayaran penuh untuk satu invoice. Pembayaran parsial belum ditetapkan sebagai bagian MVP.

Status invoice merupakan ringkasan kondisi tagihan, sedangkan status payment menunjukkan kondisi setiap percobaan transaksi.

## 8. UX requirements yang diperbarui

Tambahan pada UX Requirements bagian 11:

- Owner Portal harus membedakan pengajuan booking yang menunggu keputusan, disetujui, dan ditolak.
- Clinic Portal harus menyediakan daftar pengajuan booking yang menunggu review admin.
- Admin harus dapat melihat informasi jadwal petugas dan kapasitas layanan sebelum mengambil keputusan booking.
- Saat booking ditolak, pemilik harus memperoleh informasi status dan alasan penolakan yang telah dicatat admin.
- Owner Portal harus menampilkan status transaksi gateway secara jelas: `PENDING`, `PAID`, `FAILED`, atau `CANCELLED`.
- Invoice harus tetap menampilkan status `BELUM_DIBAYAR` selama belum terdapat pembayaran penuh yang berhasil diverifikasi.
- Sistem tidak boleh menampilkan status `PAID` atau bukti lunas sebelum hasil transaksi diverifikasi.
- Saat transaksi masih `PENDING`, sistem harus memberikan informasi bahwa pembayaran masih diproses dan mencegah inisiasi transaksi aktif yang duplikat.
- Pesan error pembayaran harus menjelaskan kondisi dan tindakan berikutnya tanpa mengungkap data sensitif provider.
- Informasi transaksi yang membutuhkan tindak lanjut harus dapat diidentifikasi oleh admin melalui Clinic Portal.

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

Event pada PRD v1.0 tetap dipertahankan dengan penjelasan berikut:

- `booking_created` dicatat ketika sistem berhasil menyimpan pengajuan booking dengan kode booking unik dan status awal `MENUNGGU_KONFIRMASI`.
- `booking_created` digunakan sebagai event awal untuk mengukur proses booking dan tidak diduplikasi dengan event pengajuan lain untuk aktivitas yang sama.
- `booking_approved` dicatat ketika admin berhasil menyetujui pengajuan booking.
- `booking_rejected` dicatat ketika admin menolak pengajuan booking.
- `payment_initiated` dicatat ketika sistem berhasil memulai percobaan pembayaran melalui gateway.
- `payment_status_updated` dicatat ketika status transaksi berubah berdasarkan hasil provider yang telah diverifikasi.
- `payment_completed` hanya dicatat ketika transaksi berhasil dan status `PAID` telah diverifikasi sistem.
- `payment_failed` dicatat ketika transaksi dinyatakan gagal oleh provider.

Event booking approval dan rejection digunakan untuk mengukur proses pengambilan keputusan admin. Event payment digunakan untuk memantau inisiasi, perubahan status, dan hasil transaksi gateway.

Notifikasi provider yang berulang tidak boleh menghasilkan pencatatan event perubahan status yang berulang untuk transaksi dan perubahan status yang sama.

### 10.2 Success metrics tambahan

Target numerik pada PRD v1.0 tidak diubah. Tambahkan metrik observasi berikut:

- Waktu dari `booking_created` sampai keputusan admin (`booking_approved` atau `booking_rejected`).
- Jumlah dan proporsi booking yang disetujui dan ditolak.
- Jumlah booking yang masih menunggu keputusan admin.
- Jumlah booking yang gagal memperoleh approval karena kapasitas tidak tersedia.
- Jumlah transaksi gateway dengan status pending, paid, failed, atau cancelled.
- Tingkat keberhasilan transaksi gateway berdasarkan transaksi yang telah diverifikasi.
- Proporsi transaksi paid yang memiliki bukti pembayaran dan referensi transaksi.
- Jumlah transaksi yang membutuhkan rekonsiliasi karena duplikasi atau ketidaksesuaian hasil pembayaran.

Target baru belum ditetapkan karena memerlukan data pilot. Target awal pada PRD v1.0 tetap merupakan target awal, bukan hasil pengukuran aktual.

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

1. Pemilik dapat mengirim pengajuan booking dan menerima kode booking unik dengan status awal `MENUNGGU_KONFIRMASI`.
2. Admin berwenang dapat melihat daftar pengajuan booking beserta detail layanan, jadwal petugas, dan kapasitas layanan.
3. Admin dapat menyetujui atau menolak pengajuan booking.
4. Sistem mencatat identitas admin, waktu keputusan, dan alasan penolakan.
5. Pemilik dapat melihat status dan hasil keputusan booking melalui Owner Portal.
6. Sistem memvalidasi ulang kapasitas sebelum approval dan mencegah kapasitas layanan terlampaui.
7. Sistem membentuk invoice berdasarkan layanan dan item yang telah dicatat sesuai proses operasional klinik.
8. Pemilik dapat melihat invoice dan memulai pembayaran melalui payment gateway yang dikonfigurasi.
9. Setiap percobaan pembayaran memiliki referensi unik dan terhubung dengan invoice yang benar.
10. Status pembayaran hanya menjadi `PAID` setelah hasil transaksi provider diverifikasi sistem.
11. Invoice hanya berubah menjadi `LUNAS` setelah pembayaran penuh berhasil diverifikasi.
12. Transaksi pending, failed, atau cancelled tidak menghasilkan bukti pembayaran lunas.
13. Sistem menerbitkan bukti pembayaran setelah transaksi berhasil diverifikasi.
14. Admin dapat memantau status transaksi dan mengidentifikasi transaksi yang membutuhkan tindak lanjut.
15. Pemrosesan notifikasi gateway berulang tidak menghasilkan pencatatan pembayaran atau penerbitan bukti pembayaran ganda.
16. Jika terdapat transaksi berbeda yang berhasil untuk invoice yang sama, sistem menandainya untuk rekonsiliasi.
17. Status booking, invoice, dan payment transaction ditampilkan sebagai lifecycle yang berbeda.

## 14. Keputusan terbuka sebelum sign-off

Keputusan berikut masih memerlukan validasi dan persetujuan stakeholder sebelum implementasi final:

**Booking dan Scheduling**
- Kapasitas maksimum layanan berdasarkan petugas, jenis layanan, dan sumber daya klinik.
- Apakah pengajuan `MENUNGGU_KONFIRMASI` menahan slot atau hanya menjadi permintaan sementara.
- Durasi penahanan slot dan aturan expiry jika mekanisme slot hold digunakan.
- SLA admin dalam memproses pengajuan booking.
- Kebijakan booking yang dibuat admin atas nama pemilik.
- Penanganan pengajuan ketika kapasitas tidak lagi tersedia saat admin melakukan approval.

**Payment dan Billing**
- Provider gateway, metode pembayaran, biaya, dan settlement.
- Apakah pembayaran tunai/manual tersedia sebagai alternatif gateway.
- Kebijakan percobaan ulang pembayaran dan batas transaksi aktif per invoice.
- Penanganan transaksi pending yang tidak memperoleh pembaruan status dari provider.
- Prosedur rekonsiliasi jika terdapat lebih dari satu transaksi berhasil untuk invoice yang sama.
- Kebijakan refund, deposit, dan pembayaran parsial.
- Pemetaan rinci status invoice terhadap status payment gateway.

**Notification dan Operations**
- Kanal notifikasi dan template pesan untuk approval/rejection booking.
- Kanal notifikasi untuk status pembayaran berhasil, gagal, atau pending.
- Prosedur penanganan transaksi yang membutuhkan tindak lanjut admin.

Keputusan tersebut harus didokumentasikan sebelum finalisasi requirement teknis dan stakeholder sign-off.

## 15. Keterlacakan dan status dokumen

| Area | Dokumen lanjutan |
|---|---|
| Booking approval/rejection | BRD business rules, SRD booking workflow, dan activity/sequence diagram booking. |
| Slot hold/expiry | BRD booking rules dan SRD scheduling/booking logic. |
| Gateway payment | BRD billing requirements, SRD payment integration, dan payment sequence/activity diagram. |
| Payment status dan receipt | SRD transaction/data requirements. |
| Product analytics | Implementasi event tracking dan dashboard reporting. |

**Status akhir:** Proposed refinement / Draft - pending stakeholder validation. PRD v1.0 tetap menjadi baseline dan tidak diubah. Addendum ini bukan bukti bahwa provider gateway, aturan operasional, atau target produk telah disetujui stakeholder.

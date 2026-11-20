# RabbitCare - MRD Refinement Addendum v1.1

**Tanggal refinement:** 20 November 2026  
**Dokumen sumber:** Market Requirements Document (MRD) RabbitCare v1.0  
**Status:** Proposed refinement / Draft - pending stakeholder validation  
**Jenis perubahan:** Addendum terhadap baseline, bukan penggantian dokumen asli

## Ringkasan keputusan

Dua keputusan arah produk menjadi dasar refinement ini:

1. **Booking memerlukan persetujuan admin.** Pemilik tetap dapat mengajukan booking secara mandiri, tetapi booking belum terkonfirmasi sampai admin menyetujui atau menolaknya.
2. **Payment gateway termasuk dalam MVP.** Pemilik dapat membayar invoice melalui integrasi payment gateway; status transaksi dan bukti pembayaran tersedia berdasarkan hasil transaksi yang telah diverifikasi sistem.

MRD v1.0 tetap dipertahankan tanpa perubahan. Addendum ini memperbarui bagian yang terdampak dan tidak menyatakan bahwa asumsi pasar atau keputusan operasional telah divalidasi oleh calon pengguna/klinik.

## 1. Tujuan dan cara membaca addendum

Addendum ini menyelaraskan kebutuhan pasar, ruang lingkup produk, pengalaman pengguna, prioritas fitur, dan strategi peluncuran RabbitCare dengan keputusan terbaru mengenai booking dan pembayaran.

Gunakan MRD v1.0 sebagai baseline. Untuk bagian yang secara eksplisit dibahas di addendum ini, rumusan v1.1 menjadi usulan terbaru untuk validasi. Bagian lain yang tidak disebutkan tetap mengikuti MRD v1.0.

## 2. Ringkasan perubahan terhadap MRD v1.0

| Bagian MRD v1.0 | Baseline | Refinement v1.1 |
|---|---|---|
| 10. Persona Admin | Admin membutuhkan dashboard booking, check-in, antrean, dan pembayaran. | Kebutuhan admin diperjelas mencakup review, approval/rejection booking, serta pemantauan status transaksi gateway. |
| 11. Jobs to Be Done | Admin ingin menemukan data booking dan pasien untuk check-in dan antrean. | Ditambahkan kebutuhan untuk meninjau permintaan booking dan memberi kepastian kepada pemilik. |
| 12. Value Proposition | RabbitCare mengintegrasikan booking, antrean, rekam medis, grooming, vaksinasi, pembayaran, dan laporan. | Nilai booking yang lebih terkelola dan pembayaran digital melalui gateway ditegaskan. |
| 14.1 Owner Module | Pemilik dapat booking, melihat status, tagihan, dan menerima reminder. | Ditambahkan visibilitas hasil keputusan admin dan pembayaran invoice melalui gateway. |
| 14.1 Admin Module | Admin mengelola jadwal, konfirmasi booking, tagihan, dan konfirmasi pembayaran. | Diperjelas menjadi approval/rejection booking dan pemantauan transaksi gateway. |
| 14.2 Out of Scope | Integrasi penuh payment gateway berada di luar tahap awal. | Integrasi payment gateway dipindahkan ke ruang lingkup MVP. |
| 15 MR-04 dan MR-15 | Booking mandiri; admin mengonfirmasi pembayaran. | Booking mandiri tetap tersedia dengan approval admin; peran admin pada pembayaran gateway berubah menjadi pemantauan dan penanganan pengecualian. |
| 16 MoSCoW | Pembayaran Must Have; bukti pembayaran digital Should Have. | Integrasi gateway dan bukti pembayaran digital menjadi Must Have. |
| 17.2 Booking Journey | Booking disimpan, nomor booking dibuat, konfirmasi dikirim. | Ditambahkan status menunggu keputusan admin, approval/rejection, dan notifikasi hasil keputusan. |
| 17.5 Payment Journey | Pemilik memilih metode, pembayaran diproses, status diperbarui, bukti diterbitkan. | Alur secara eksplisit menggunakan gateway dan hasil transaksi terverifikasi. |
| 22. Success Metrics | Target seluruh pembayaran layanan memiliki bukti transaksi. | Kriteria tersebut dipertahankan dan dikaitkan dengan referensi transaksi gateway. |
| 27. Product Launch | Pembayaran digital dikembangkan setelah MVP. | Integrasi payment gateway dipindahkan ke MVP; pengembangan lanjutan tetap dapat mencakup fitur pembayaran yang belum diputuskan. |
| 29. Market Validation | Menanyakan apakah klinik membutuhkan pembayaran digital. | Validasi diarahkan pada provider, metode, biaya, alternatif tunai/manual, dan penanganan transaksi. |

## 3. Penyesuaian persona dan Jobs to Be Done

### 3.1 Pemilik kelinci

Kebutuhan persona pemilik tetap mengikuti MRD v1.0: profil kelinci, booking, reminder, riwayat kesehatan, dan informasi tagihan.

Kebutuhan yang diperjelas:
- Mengetahui bahwa booking telah diterima sebagai permintaan, bukan langsung dianggap terkonfirmasi.
- Melihat status keputusan admin dan informasi penolakan jika booking ditolak.
- Membayar invoice melalui payment gateway.
- Melihat status transaksi dan bukti pembayaran setelah transaksi berhasil.

### 3.2 Admin klinik

Kebutuhan admin diperluas dari dashboard booking, check-in, antrean, dan pembayaran menjadi:
- Meninjau permintaan booking yang diajukan pemilik.
- Menyetujui atau menolak booking dan menyampaikan hasilnya.
- Mengelola jadwal dan proses check-in/antrean seperti yang telah ditetapkan pada MRD v1.0.
- Memantau status transaksi gateway dan menindaklanjuti transaksi yang memerlukan perhatian.
- Tidak menetapkan pembayaran gateway sebagai berhasil hanya berdasarkan klaim atau bukti yang dikirim pemilik; status sukses mengikuti hasil provider yang diverifikasi sistem.

### 3.3 Jobs to Be Done yang diperbarui

**Pemilik kelinci:** Ketika ingin menjadwalkan layanan, pemilik ingin mengajukan booking dan mengetahui hasil persetujuan admin agar dapat memperoleh kepastian jadwal. Ketika menerima invoice, pemilik ingin membayar melalui gateway dan melihat status serta bukti transaksi.

**Admin klinik:** Ketika menerima permintaan booking, admin ingin meninjau ketersediaan dan kebutuhan layanan lalu menyetujui atau menolak permintaan agar jadwal klinik tetap terkelola. Ketika pembayaran dilakukan melalui gateway, admin ingin memantau status transaksi dan menangani pengecualian.

## 4. Penyesuaian ruang lingkup produk

### 4.1 Modul Pemilik - termasuk MVP

- Registrasi dan login.
- Pengelolaan profil pemilik dan beberapa profil kelinci.
- Pencarian jadwal layanan.
- Pengajuan booking pemeriksaan, vaksinasi, dan grooming.
- Melihat status booking, termasuk menunggu konfirmasi, terkonfirmasi, atau ditolak.
- Melihat riwayat kesehatan, vaksin, dan grooming.
- Melihat tagihan.
- Memulai pembayaran invoice melalui payment gateway.
- Melihat status transaksi dan bukti pembayaran setelah transaksi berhasil.
- Menerima reminder.

### 4.2 Modul Admin - termasuk MVP

- Login admin.
- Pengelolaan data pemilik dan pasien kelinci.
- Pengelolaan jadwal dokter dan groomer.
- Meninjau, menyetujui, atau menolak booking.
- Melihat dan mengelola check-in serta antrean.
- Mengelola tagihan dan invoice.
- Memantau status transaksi payment gateway dan menangani pengecualian operasional.

### 4.3 Penyesuaian batasan tahap awal

Pernyataan "integrasi penuh dengan payment gateway" pada bagian 14.2 MRD v1.0 tidak lagi menjadi item di luar ruang lingkup tahap awal. Integrasi gateway untuk pembayaran invoice merupakan bagian MVP sesuai keputusan produk.

Hal-hal berikut belum ditetapkan sebagai bagian MVP dan memerlukan keputusan terpisah:
- Provider gateway tertentu dan penggunaan lebih dari satu provider.
- Pembayaran deposit atau pembayaran sebelum layanan.
- Pembayaran parsial.
- Refund otomatis.
- Pembayaran tunai/manual sebagai alternatif di samping gateway.
- Fitur rekonsiliasi lanjutan dan penanganan sengketa.

Daftar ini adalah batas keputusan yang belum ditetapkan, bukan hasil validasi pasar.

## 5. Kebutuhan pasar yang diperbarui

MR-01 sampai MR-20 tetap menjadi baseline. Kebutuhan berikut ditambahkan untuk mencerminkan keputusan terbaru.

| ID | Kebutuhan Pasar | Prioritas |
|---|---|---|
| MR-21 | Admin dapat meninjau, menyetujui, atau menolak booking yang diajukan pemilik, dan pemilik dapat mengetahui hasil keputusannya. | Must Have |
| MR-22 | Pemilik dapat membayar invoice melalui payment gateway yang terintegrasi dan melihat status transaksi serta bukti pembayaran. | Must Have |
| MR-23 | Klinik dapat memantau status transaksi gateway dan mengidentifikasi transaksi yang memerlukan tindak lanjut. | Must Have |

### Penyesuaian MR-04 dan MR-15

- **MR-04:** Pemilik dapat mengajukan booking secara mandiri. Pengajuan tersebut menunggu keputusan admin sebelum menjadi booking terkonfirmasi.
- **MR-15:** Admin dapat memantau status pembayaran gateway dan menangani pengecualian operasional. Status sukses transaksi gateway berasal dari hasil provider yang diverifikasi sistem, bukan konfirmasi manual admin.

MR-19 tentang penyimpanan status booking secara terstruktur tetap berlaku dan perlu mencakup status keputusan admin.

## 6. Penyesuaian prioritas fitur (MoSCoW)

### 6.1 Must Have - penyesuaian

Tambahan atau penegasan Must Have:
- Pengajuan booking oleh pemilik.
- Review, approval, dan rejection booking oleh admin.
- Tampilan status keputusan booking bagi pemilik.
- Integrasi payment gateway untuk pembayaran invoice.
- Pemantauan status transaksi gateway.
- Bukti pembayaran digital untuk transaksi berhasil.

Fitur Must Have lain pada MRD v1.0 tetap berlaku.

### 6.2 Should Have - tetap atau ditinjau

Fitur berikut tetap mengikuti baseline MRD v1.0:
- Rujukan groomer ke dokter.
- Reschedule booking.
- Pembatalan booking.
- Status no-show.
- Dashboard laporan.
- Retry pengiriman notifikasi.
- Pencatatan perubahan data.

Pengiriman bukti pembayaran digital dipindahkan dari Should Have ke Must Have untuk transaksi gateway yang berhasil.

### 6.3 Could Have dan Won't Have

Kategori Could Have dan Won't Have lain tetap mengikuti MRD v1.0. Fitur lanjutan pembayaran seperti deposit, pembayaran parsial, refund otomatis, atau multi-provider tidak otomatis masuk MVP; fitur tersebut memerlukan keputusan dan validasi terpisah.

## 7. User journey yang diperbarui

### 7.1 Alur booking

1. Pemilik memilih profil kelinci.
2. Pemilik memilih layanan.
3. Pemilik memilih tanggal.
4. Sistem menampilkan jadwal yang tersedia.
5. Pemilik memilih slot.
6. Sistem memeriksa ulang ketersediaan.
7. Sistem menjalankan mekanisme pengamanan slot sesuai aturan booking.
8. Booking disimpan sebagai permintaan dengan status `MENUNGGU_KONFIRMASI`.
9. Nomor booking dibuat dan pemilik menerima pemberitahuan bahwa permintaan telah diterima.
10. Admin meninjau permintaan booking.
11. Jika disetujui, status menjadi `TERKONFIRMASI` dan pemilik menerima notifikasi.
12. Jika ditolak, status menjadi `DITOLAK`; pemilik menerima pemberitahuan hasil keputusan.

**Catatan:** MRD v1.0 telah menyebut penguncian slot pada alur booking. Kebijakan berapa lama slot ditahan selama menunggu admin, serta kapan slot dilepaskan, masih perlu divalidasi dengan klinik. Booking yang dibuat admin atas nama pemilik juga memerlukan aturan tersendiri.

### 7.2 Alur pembayaran

1. Admin membuka dan memeriksa tagihan sesuai proses klinik.
2. Invoice tersedia bagi pemilik.
3. Pemilik memilih untuk membayar melalui payment gateway.
4. Sistem mengarahkan atau membuat sesi pembayaran melalui provider.
5. Provider memproses transaksi.
6. Sistem menerima dan memverifikasi hasil transaksi dari provider.
7. Status pembayaran dan invoice diperbarui sesuai hasil yang tervalidasi.
8. Bukti pembayaran diterbitkan untuk transaksi berhasil.
9. Admin dapat memantau status transaksi dan menindaklanjuti pengecualian.

Transaksi gagal atau masih menunggu tidak boleh ditampilkan sebagai pembayaran berhasil. Detail status, retry, dan rekonsiliasi perlu dijabarkan pada PRD/SRD.

## 8. Penyesuaian indikator keberhasilan

Indikator adopsi, penggunaan, operasional, dan retensi pada MRD v1.0 tetap berlaku.

Indikator tambahan yang dapat diamati:
- Jumlah dan proporsi booking yang menunggu keputusan admin.
- Waktu antara pengajuan booking dan keputusan admin.
- Jumlah booking yang disetujui dan ditolak.
- Jumlah transaksi gateway berhasil, gagal, atau masih menunggu.
- Proporsi transaksi gateway berhasil yang memiliki referensi dan bukti pembayaran.

Target numerik baru tidak ditetapkan dalam addendum ini. Target awal pada MRD v1.0 tetap merupakan hipotesis produk yang perlu disesuaikan setelah pilot, bukan hasil pengukuran aktual.

## 9. Penyesuaian strategi peluncuran produk

### Tahap 1 - Validasi Internal
Tetap mencakup validasi alur dengan pemilik klinik, dokter, pet care staff, dan admin. Validasi tambahan:
- Proses persetujuan dan penolakan booking.
- Provider gateway dan metode pembayaran yang dibutuhkan.
- Penanganan transaksi pending, gagal, dan notifikasi provider yang terlambat.

### Tahap 2 - Prototype
Prototype booking perlu memperlihatkan status menunggu persetujuan, hasil persetujuan, dan hasil penolakan. Prototype pembayaran perlu memperlihatkan pemilihan gateway, status transaksi, dan bukti pembayaran.

### Tahap 3 - Minimum Viable Product
MVP mencakup fitur baseline yang telah ditetapkan pada MRD v1.0, ditambah:
- Approval/rejection booking oleh admin.
- Integrasi payment gateway untuk pembayaran invoice.
- Pemantauan status transaksi dan bukti pembayaran digital.

### Tahap 4 - Pilot Testing
Pilot perlu menguji alur booking dari pengajuan sampai keputusan admin serta alur pembayaran gateway dari inisiasi sampai status transaksi dan bukti pembayaran.

### Tahap 5 - Evaluasi dan Pengembangan
Pengembangan lanjutan dapat mencakup reschedule, pembatalan, laporan yang lebih lengkap, ekspansi cabang, dan fitur pembayaran tambahan setelah kebutuhan divalidasi.

## 10. Dependensi, risiko, dan validasi pasar

### 10.1 Dependensi tambahan
- Klinik memilih provider gateway dan menyediakan akun merchant serta akses integrasi.
- Provider mendukung metode pembayaran yang dibutuhkan dan mekanisme status transaksi yang dapat diverifikasi.
- Klinik memiliki koneksi internet yang memadai.
- Admin memiliki proses dan kapasitas untuk meninjau booking.
- Pemilik memiliki akses ke metode pembayaran yang didukung provider.

### 10.2 Risiko tambahan

| Risiko | Dampak | Area validasi/mitigasi |
|---|---|---|
| Admin terlambat meninjau booking | Pemilik belum mendapat kepastian jadwal. | Tentukan SLA review dan eskalasi. |
| Slot tertahan terlalu lama | Ketersediaan jadwal berkurang. | Validasi aturan hold dan expiry slot. |
| Gateway tidak tersedia | Pembayaran tidak dapat diselesaikan sementara. | Tentukan prosedur retry atau alternatif. |
| Status provider terlambat diterima | Status di sistem belum mutakhir. | Tentukan monitoring dan rekonsiliasi. |
| Metode gateway tidak sesuai kebutuhan pemilik | Adopsi pembayaran digital rendah. | Validasi preferensi metode dan akses pengguna. |

### 10.3 Pertanyaan validasi pasar yang diperbarui
1. Provider payment gateway apa yang dapat digunakan oleh klinik?
2. Metode pembayaran apa yang paling dibutuhkan pemilik?
3. Apakah klinik tetap akan menerima pembayaran tunai/manual sebagai alternatif?
4. Siapa yang memantau transaksi pending atau gagal?
5. Bagaimana klinik menangani refund dan pembatalan?
6. Berapa lama admin dapat memberikan keputusan booking?
7. Bagaimana kebijakan slot jika booking belum ditinjau admin?

## 11. Keputusan yang masih terbuka

Sebelum MRD dianggap disetujui stakeholder, hal berikut perlu divalidasi:
- Provider gateway, metode pembayaran, biaya, dan proses settlement.
- Ketersediaan pembayaran tunai/manual sebagai alternatif.
- SLA admin untuk approval booking.
- Batas waktu penahanan slot dan expiry booking.
- Aturan booking yang dibuat admin atas nama pemilik.
- Refund, pembayaran parsial/deposit, dan penanganan transaksi pending/gagal.
- Kebutuhan retry dan rekonsiliasi transaksi.

## 12. Keterlacakan terhadap dokumen dan proses lain

| Area MRD | Dokumen/proses terkait |
|---|---|
| MR-04, MR-19, MR-21 | BRD booking requirements dan booking business rules |
| MR-15, MR-22, MR-23 | BRD billing/payment requirements |
| Booking user journey | PRD booking flow dan SRD booking workflow |
| Payment user journey | PRD billing/payment flow dan SRD gateway integration |
| MVP scope | PRD release plan dan SRD functional requirements |

## 13. Status dokumen

MRD v1.0 tetap menjadi baseline sumber dan tidak diubah. Addendum v1.1 merupakan usulan refinement untuk menyelaraskan kebutuhan pasar dan pengalaman pengguna dengan keputusan booking admin approval serta payment gateway pada MVP.

**Status akhir:** Proposed refinement / Draft - pending stakeholder validation. Dokumen ini belum merupakan hasil validasi pasar atau persetujuan resmi pihak klinik.

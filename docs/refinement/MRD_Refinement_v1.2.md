# RabbitCare — MRD Refinement Addendum v1.2

**Tanggal refinement:** 23 November 2026  
**Dokumen sumber:** MRD RabbitCare v1.0 dan MRD Refinement Addendum v1.1  
**Dokumen penyelarasan:** Gap Analysis Refinement v1.1, PRD Refinement v1.1, dan SRD Refinement v1.2  
**Status:** Proposed refinement / Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum lanjutan; bukan penggantian dokumen baseline atau addendum sebelumnya

## Ringkasan keputusan refinement

Addendum MRD v1.2 memperjelas kebutuhan pasar dan operasional pada empat area:

1. Admin perlu memperoleh kepastian kapasitas ketika meninjau pengajuan booking. Sistem harus memvalidasi ulang kapasitas sebelum approval.
2. Kebijakan apakah booking `MENUNGGU_KONFIRMASI` menahan slot secara persisten tetap menjadi open decision. Addendum ini tidak menetapkan durasi hold atau expiry.
3. Klinik perlu memantau status Visit secara terpisah dari status setiap Service Execution agar progres kunjungan dan pelaksanaan layanan tidak tercampur.
4. Klinik perlu menelusuri identitas, peran, dan kanal/sumber pembuat booking untuk membedakan pengajuan owner dari booking yang dibuat admin.

MRD v1.0 dan MRD Refinement v1.1 tetap dipertahankan. Addendum v1.2 hanya memperbarui area yang disebutkan secara eksplisit.

> **Catatan validasi:** kebutuhan dan prioritas dalam addendum ini merupakan usulan analisis yang masih memerlukan validasi klinik. Tidak ada klaim bahwa kebutuhan pasar telah diuji kepada pengguna.

## 1. Tujuan dan cara membaca addendum

MRD v1.0 tetap menjadi baseline. MRD Refinement v1.1 merupakan refinement sebelumnya untuk owner-submitted booking dengan admin approval dan payment gateway dalam MVP. Addendum v1.2 melengkapi kebutuhan tersebut dengan validasi kapasitas, pemisahan Visit/Service Execution, dan traceability sumber booking.

Jika terdapat perbedaan pada area yang secara eksplisit dibahas di sini, gunakan rumusan v1.2 sebagai usulan terbaru untuk validasi. Bagian lain tetap mengikuti MRD v1.0 dan addendum v1.1.

## 2. Ringkasan perubahan terhadap MRD v1.1

| Area | Ketentuan v1.1 | Klarifikasi v1.2 |
|---|---|---|
| Booking review | Admin meninjau dan memutuskan pengajuan owner. | Sistem memvalidasi ulang kapasitas sebelum approval; approval tidak dapat menghasilkan overbooking. |
| Pending slot | Kebijakan hold/expiry belum ditetapkan. | Diperjelas bahwa persistent slot hold tetap open decision; pemeriksaan kapasitas saat approval tetap wajib. |
| Visit | Check-in dan antrean telah tercakup. | Status Visit digunakan untuk memantau progres kunjungan pasien. |
| Service Execution | Catatan hasil layanan telah tercakup. | Status setiap pelaksanaan layanan dicatat terpisah dari Visit dan layanan lain. |
| Booking source | Booking owner dan booking admin memerlukan aturan terpisah. | Identitas, peran, dan kanal/sumber pembuat booking harus dapat ditelusuri. |
| Lifecycle | Booking, invoice, dan payment telah diperjelas. | Booking, Visit, Service Execution, Invoice, dan Payment diperlakukan sebagai lifecycle terpisah. |

## 3. Kebutuhan pasar yang diperbarui

### 3.1 Klarifikasi MR-21

MR-21 tetap berlaku dengan klarifikasi berikut:

Admin harus dapat meninjau pengajuan booking owner, melihat jadwal petugas dan kapasitas layanan, lalu menyetujui atau menolak pengajuan. Sebelum approval disimpan, sistem memvalidasi ulang kapasitas. Jika kapasitas tidak lagi tersedia, approval tidak dapat dilanjutkan dan admin memperoleh informasi untuk menindaklanjuti pengajuan.

### 3.2 Kebutuhan pasar tambahan

| ID | Kebutuhan Pasar | Prioritas Usulan |
|---|---|---|
| MR-24 | Klinik dapat memantau status kunjungan pasien (Visit) secara terpisah dari status setiap pelaksanaan layanan (Service Execution) dalam kunjungan terkait. | Must Have — proposed |
| MR-25 | Klinik dapat menelusuri identitas, peran, dan kanal/sumber pembuat booking untuk membedakan pengajuan yang dibuat pemilik dari booking yang dibuat admin. | Must Have — proposed |

Prioritas MR-24 dan MR-25 merupakan usulan untuk MVP karena mendukung keterlacakan proses operasional dan mencegah pencampuran status. Prioritas akhir tetap memerlukan persetujuan stakeholder.

## 4. Penyesuaian kebutuhan pengguna dan modul

### 4.1 Admin Klinik

Kebutuhan admin diperjelas menjadi:
- Meninjau permintaan booking owner dan melihat jadwal petugas serta kapasitas layanan.
- Memperoleh validasi kapasitas terbaru sebelum approval disimpan.
- Mengetahui sumber dan pembuat booking.
- Memantau status Visit dan antrean pasien.
- Melihat status pelaksanaan setiap layanan secara terpisah.
- Memantau invoice dan transaksi gateway sesuai hak akses yang ditetapkan.

### 4.2 Dokter dan Groomer

Kebutuhan pengguna klinis diperjelas menjadi:
- Mengakses Visit dan riwayat pasien yang relevan sesuai hak akses.
- Melihat pelaksanaan layanan yang menjadi tanggung jawabnya.
- Mencatat progres dan hasil Service Execution tanpa mengubah status Visit atau layanan lain secara otomatis.

Pembagian hak untuk memperbarui status final Visit dan Service Execution masih perlu divalidasi bersama klinik.

### 4.3 Pemilik Kelinci

Kebutuhan pemilik pada MRD v1.1 tetap berlaku: mengajukan booking, melihat keputusan admin, mengakses riwayat, melihat invoice, dan memulai pembayaran gateway. Addendum ini tidak menambahkan klaim bahwa pemilik harus dapat melihat seluruh status internal Visit atau Service Execution; kebutuhan tampilan kepada pemilik perlu ditentukan melalui validasi UX dan stakeholder.

## 5. Penyesuaian user journey

### 5.1 Booking request dan admin decision

1. Pemilik memilih profil kelinci, layanan, tanggal, dan slot.
2. Sistem menampilkan opsi berdasarkan jadwal dan kapasitas yang tersedia.
3. Sistem memvalidasi kapasitas sebelum menyimpan pengajuan.
4. Pengajuan owner disimpan dengan status `MENUNGGU_KONFIRMASI` dan nomor booking unik.
5. Admin meninjau detail pengajuan, jadwal petugas, dan kapasitas layanan.
6. Sistem memvalidasi ulang kapasitas sebelum approval.
7. Jika kapasitas masih tersedia, admin dapat menyetujui pengajuan.
8. Jika kapasitas tidak tersedia, approval diblokir dan admin memperoleh informasi untuk tindak lanjut.
9. Pemilik dapat melihat status dan hasil keputusan admin.

**Catatan pending slot:** apakah pengajuan `MENUNGGU_KONFIRMASI` menahan kapasitas secara persisten, durasi hold, dan expiry tetap merupakan keputusan terbuka. MRD ini tidak menetapkan salah satu kebijakan tersebut.

### 5.2 Visit dan Service Execution

1. Pasien datang sesuai booking yang telah terkonfirmasi dan menjalani proses check-in.
2. Klinik mencatat Visit untuk menelusuri kehadiran dan progres kunjungan pasien.
3. Antrean dikelola sesuai proses klinik.
4. Setiap layanan yang dilakukan dicatat sebagai Service Execution dengan status dan waktu pelaksanaan tersendiri.
5. Perubahan status Service Execution tidak otomatis mengubah status Visit atau layanan lainnya.
6. Hasil layanan ditautkan ke profil kelinci agar riwayat perawatan dapat ditelusuri.

Waktu pembentukan Visit, jumlah Service Execution dalam satu Visit, dan nama status/transisinya masih memerlukan validasi stakeholder.

### 5.3 Pembayaran

Alur pembayaran gateway pada MRD v1.1 tetap berlaku. Status Payment dan Invoice tidak digunakan untuk menggantikan atau mengubah status Booking, Visit, maupun Service Execution.

## 6. Penyesuaian ruang lingkup MVP dan prioritas

Usulan kemampuan MVP yang diperjelas:
- Review booking owner dan validasi ulang kapasitas sebelum approval.
- Pemantauan Visit secara terpisah dari Booking dan Service Execution.
- Pencatatan status masing-masing Service Execution.
- Traceability identitas, peran, dan kanal/sumber pembuat booking.
- Pemisahan lifecycle Booking, Visit, Service Execution, Invoice, dan Payment.

MR-24 dan MR-25 diusulkan sebagai Must Have karena mendukung keterlacakan operasional. Penetapan prioritas akhir memerlukan persetujuan stakeholder. Fitur lain yang tidak dibahas tetap mengikuti prioritas MRD v1.0 dan MRD Refinement v1.1.

## 7. Penyesuaian indikator keberhasilan

Indikator tambahan yang dapat diamati selama pilot:
- Setiap pengajuan booking owner memiliki status dan sumber pembuat yang dapat ditelusuri.
- Setiap approval booking didahului validasi ulang kapasitas.
- Tidak ada booking yang dikonfirmasi ketika kapasitas tidak tersedia.
- Setiap Visit memiliki catatan status kunjungan yang dapat ditelusuri.
- Setiap Service Execution memiliki status dan waktu pelaksanaan tersendiri.
- Perubahan status satu Service Execution tidak mengubah status Visit atau layanan lain.
- Status Payment dan Invoice dapat ditelusuri tanpa mengubah status Booking atau Visit.

Target numerik tidak ditetapkan dalam addendum ini. Target pada MRD v1.0 tetap merupakan hipotesis produk yang perlu dievaluasi melalui pilot, bukan hasil pengukuran aktual.

## 8. Kriteria penerimaan pasar awal tambahan

1. Admin dapat melihat jadwal petugas dan kapasitas layanan sebelum mengambil keputusan booking.
2. Sistem memvalidasi ulang kapasitas sebelum approval.
3. Approval diblokir jika kapasitas tidak lagi tersedia.
4. Klinik dapat membedakan pengajuan owner dari booking yang dibuat admin melalui identitas, peran, dan kanal/sumber pembuat booking.
5. Status Visit dapat dipantau secara terpisah dari status Service Execution.
6. Setiap Service Execution memiliki status dan waktu pelaksanaan tersendiri.
7. Perubahan status satu Service Execution tidak otomatis mengubah status Visit atau layanan lain.
8. Perubahan status Payment dan Invoice tidak mengubah status Booking atau Visit.

Kriteria ini masih memerlukan validasi stakeholder dan pengujian pada pilot.

## 9. Keputusan yang masih terbuka

1. Apakah booking `MENUNGGU_KONFIRMASI` menahan slot secara persisten.
2. Durasi hold, expiry, dan SLA admin jika mekanisme hold digunakan.
3. Penanganan pengajuan jika kapasitas tidak tersedia saat approval.
4. Kebijakan booking yang dibuat admin atas nama pemilik.
5. Kapan Visit dibuat dalam alur check-in/kunjungan.
6. Apakah satu Visit dapat memiliki beberapa Service Execution.
7. Nama status dan transisi final Visit serta Service Execution.
8. Peran yang berwenang memperbarui status Visit dan Service Execution.
9. Informasi Visit/Service Execution apa yang perlu ditampilkan kepada pemilik, jika diperlukan.

## 10. Keterlacakan dan penyelarasan dokumen

| Kebutuhan pasar | Dokumen terkait |
|---|---|
| MR-21 — admin review dan capacity revalidation | BR-53, BR-57; PR-14, PR-15, PR-59; SR-F-085–092 |
| MR-24 — Visit dan Service Execution terpisah | BR-58, BR-59; SR-F-101–102 |
| MR-25 — sumber dan pembuat booking dapat ditelusuri | BR-60; SR-F-103 |

PRD Refinement v1.1 telah mencakup capacity revalidation dan pemisahan lifecycle Booking/Invoice/Payment. Product requirements untuk MR-24 dan MR-25 masih perlu ditambahkan pada tahap refinement PRD berikutnya. Addendum ini tidak menetapkan ID PRD baru secara sepihak.

## 11. Status dokumen

MRD v1.0 tetap menjadi baseline dan MRD Refinement v1.1 tetap tersedia sebagai riwayat refinement sebelumnya. Addendum v1.2 memperjelas kebutuhan kapasitas booking, pemantauan Visit/Service Execution, dan traceability sumber booking.

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation. Dokumen ini belum merupakan hasil validasi pasar atau persetujuan resmi pihak klinik.

# RabbitCare — BRD Refinement Addendum v1.2

**Tanggal refinement:** 23 November 2026  
**Dokumen sumber:** BRD RabbitCare v1.0 dan BRD Refinement Addendum v1.1  
**Dokumen penyelarasan:** Gap Analysis Refinement v1.1, PRD Refinement v1.1, dan SRD Refinement v1.2  
**Status:** Proposed refinement / Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum lanjutan; bukan penggantian dokumen baseline atau addendum sebelumnya

## Ringkasan keputusan refinement

Addendum v1.2 memperjelas empat area agar kebutuhan bisnis selaras dengan model produk dan sistem terbaru:

1. **Capacity revalidation saat approval.** Sistem harus memeriksa kembali kapasitas jadwal, petugas, dan layanan sebelum admin menyetujui booking yang diajukan pemilik. Booking tidak boleh dikonfirmasi jika kapasitas sudah tidak tersedia.
2. **Pending slot hold tetap menjadi open decision.** Addendum ini tidak menetapkan apakah permintaan `MENUNGGU_KONFIRMASI` menahan slot secara persisten. Pemeriksaan ketersediaan saat pengajuan dan approval tetap wajib, sedangkan durasi hold dan expiry memerlukan validasi stakeholder.
3. **Visit dan Service Execution dipisahkan.** Status kunjungan pasien dicatat terpisah dari status pelaksanaan setiap layanan yang dilakukan selama kunjungan.
4. **Lifecycle dan sumber booking dapat ditelusuri.** Booking, Visit, Service Execution, Invoice, dan Payment merupakan lifecycle yang berbeda. Sistem juga mencatat identitas, peran, dan kanal/sumber pembuat booking.

BRD v1.0 tetap menjadi baseline yang dipertahankan. Addendum v1.1 tetap tersedia sebagai riwayat refinement sebelumnya. Addendum v1.2 hanya memperjelas bagian yang disebutkan di sini.

> **Catatan validasi:** seluruh perubahan dalam addendum ini merupakan usulan analisis. Addendum ini bukan bukti persetujuan klinik atau hasil validasi operasional.

## 1. Tujuan dan cara membaca addendum

Gunakan BRD v1.0 sebagai baseline, BRD Refinement v1.1 sebagai refinement sebelumnya, dan addendum v1.2 ini untuk area yang secara eksplisit diperbarui.

Jika rumusan v1.1 mengenai penahanan slot selama booking menunggu approval dibaca sebagai usulan slot hold persisten, usulan tersebut **tidak dianggap sebagai aturan final**. Kebijakan apakah slot ditahan setelah transaksi pengajuan selesai tetap terbuka sampai divalidasi stakeholder.

Ketentuan lain yang tidak dibahas dalam addendum ini tetap mengikuti BRD v1.0 dan addendum v1.1.

## 2. Ringkasan perubahan terhadap refinement sebelumnya

| Area | Ketentuan sebelumnya | Klarifikasi v1.2 |
|---|---|---|
| Booking approval | Admin menyetujui atau menolak pengajuan owner. | Kapasitas wajib divalidasi ulang sebelum approval disimpan. |
| Pending slot | v1.1 mencatat proposed control bahwa slot pending tetap ditahan. | Persistent slot hold, durasi, dan expiry tetap menjadi open decision; tidak diasumsikan sebagai aturan final. |
| Visit | Check-in dan antrean tercantum dalam proses bisnis. | Status Visit dicatat sebagai progres kunjungan pasien, terpisah dari status pelaksanaan layanan. |
| Service Execution | Hasil layanan dicatat setelah pemeriksaan/grooming. | Setiap pelaksanaan layanan memiliki status dan progres tersendiri dalam Visit terkait. |
| Lifecycle | Booking, tagihan, dan pembayaran telah diperjelas. | Booking, Visit, Service Execution, Invoice, dan Payment ditegaskan sebagai lifecycle terpisah. |
| Booking traceability | Perubahan data penting mencatat pengguna dan timestamp. | Identitas, peran, serta kanal/sumber saat booking dibuat juga harus dapat ditelusuri. |

## 3. Booking approval dan validasi kapasitas

### 3.1 Alur bisnis yang diperjelas

1. Pemilik memilih profil kelinci, layanan, tanggal, dan slot yang tersedia.
2. Sistem memeriksa jadwal, ketersediaan petugas, dan kapasitas layanan sebelum menerima pengajuan.
3. Sistem menyimpan pengajuan owner dengan status `MENUNGGU_KONFIRMASI`.
4. Admin meninjau detail pengajuan, jadwal petugas, dan kapasitas layanan.
5. Sebelum approval disimpan, sistem memvalidasi ulang kapasitas pada jadwal terkait.
6. Jika kapasitas masih tersedia, admin dapat menyetujui pengajuan dan status berubah menjadi `TERKONFIRMASI`.
7. Jika kapasitas tidak lagi tersedia, sistem harus menolak aksi approval dan menampilkan informasi kepada admin. Pengajuan tidak boleh berubah menjadi `TERKONFIRMASI`.
8. Admin menindaklanjuti pengajuan sesuai kebijakan yang disepakati klinik. Addendum ini tidak menetapkan secara sepihak apakah tindak lanjut harus berupa penolakan, perubahan jadwal, atau tindakan lain.

### 3.2 Business requirement tambahan

| ID | Business Requirement | Prioritas |
|---|---|---|
| BR-57 | Sistem harus memvalidasi ulang ketersediaan jadwal, petugas, dan kapasitas layanan sebelum admin menyetujui booking yang diajukan pemilik. Approval tidak boleh menghasilkan booking terkonfirmasi yang melebihi kapasitas. | Must |

### 3.3 Kebijakan pending slot hold

Pemeriksaan kapasitas pada saat pengajuan dan validasi ulang saat approval merupakan kontrol yang wajib. Namun, hal tersebut tidak otomatis berarti kapasitas harus ditahan terus-menerus selama status booking `MENUNGGU_KONFIRMASI`.

Keputusan berikut masih terbuka:
- Apakah pengajuan pending langsung menahan kapasitas setelah permintaan tersimpan.
- Jika slot ditahan, berapa lama penahanan berlaku.
- Kapan pengajuan pending kedaluwarsa dan kapasitas dilepaskan.
- Bagaimana pengajuan ditindaklanjuti ketika kapasitas tidak lagi tersedia pada saat approval.
- Berapa SLA admin untuk meninjau pengajuan.

## 4. Visit dan Service Execution

### 4.1 Visit — kunjungan pasien

Visit merepresentasikan satu kunjungan pasien yang dapat ditelusuri melalui profil kelinci dan booking terkait. Status Visit digunakan untuk menggambarkan kehadiran dan progres kunjungan pasien, bukan untuk menggantikan status setiap layanan.

### 4.2 Service Execution — pelaksanaan layanan

Service Execution merepresentasikan pelaksanaan suatu layanan yang dicatat dalam Visit terkait. Setiap pelaksanaan layanan memiliki status dan waktu pelaksanaan tersendiri. Perubahan status satu pelaksanaan layanan tidak otomatis mengubah status Visit atau pelaksanaan layanan lainnya.

Nama status, transisi, waktu pembentukan Visit, dan jumlah pelaksanaan layanan yang dapat dicatat dalam satu Visit masih memerlukan validasi stakeholder.

### 4.3 Business requirements tambahan

| ID | Business Requirement | Prioritas |
|---|---|---|
| BR-58 | Sistem harus mencatat status kunjungan pasien (Visit) secara terpisah dari status booking dan status pelaksanaan layanan. | Must |
| BR-59 | Sistem harus mencatat status setiap pelaksanaan layanan (Service Execution) secara terpisah dari status Visit dan pelaksanaan layanan lainnya dalam kunjungan terkait. | Must |

## 5. Pemisahan lifecycle dan business rule

Booking, Visit, Service Execution, Invoice, dan Payment memiliki tujuan pencatatan yang berbeda dan harus dapat ditelusuri secara terpisah.

Contoh penerapannya:
- Perubahan status Payment tidak mengubah status Booking atau Visit.
- Invoice menjadi `LUNAS` hanya setelah pembayaran penuh berhasil diverifikasi sesuai BRule-10.
- Perubahan status Service Execution tidak otomatis mengubah status Visit.
- Perubahan status satu Service Execution tidak mengubah status Service Execution lainnya.
- Relasi antar-record tetap dipertahankan agar perjalanan pasien dan transaksi dapat ditelusuri.

| ID | Business Rule | Status |
|---|---|---|
| BRule-17 | Status Booking, Visit, Service Execution, Invoice, dan Payment merepresentasikan lifecycle yang berbeda. Perubahan pada satu lifecycle tidak otomatis mengubah lifecycle lain kecuali melalui aturan bisnis yang ditetapkan secara eksplisit. | Baru |

BRule-10, BRule-15, dan BRule-16 dari BRD Refinement v1.1 tetap berlaku.

## 6. Traceability sumber booking

Untuk mendukung review booking dan audit, sistem harus dapat membedakan pengajuan yang dibuat langsung oleh pemilik dari booking yang dibuat oleh admin atas nama pemilik.

| ID | Business Requirement | Prioritas |
|---|---|---|
| BR-60 | Sistem harus mencatat identitas pengguna, peran, serta kanal/sumber ketika booking dibuat agar asal pembuatan booking dapat ditelusuri. | Must |

Pencatatan metadata ini tidak menetapkan apakah booking yang dibuat admin harus melalui approval. Kebijakan tersebut tetap menjadi open decision.

## 7. Penyesuaian business process target

Alur target yang diperjelas:

Registrasi pemilik  
→ Pembuatan profil kelinci  
→ Pemilihan layanan  
→ Pengecekan jadwal dan kapasitas  
→ Pengajuan booking oleh pemilik  
→ Review dan keputusan admin  
→ Validasi ulang kapasitas sebelum approval  
→ Booking terkonfirmasi  
→ Kedatangan dan check-in  
→ Pencatatan Visit dan antrean  
→ Pencatatan setiap Service Execution  
→ Penyimpanan hasil layanan  
→ Pembuatan dan penerbitan invoice  
→ Pembayaran melalui gateway  
→ Verifikasi hasil transaksi  
→ Pembaruan Payment dan Invoice  
→ Penyimpanan riwayat dan reminder

Waktu pembentukan Visit dan transisi status Visit/Service Execution harus mengikuti proses operasional yang divalidasi stakeholder. Urutan ini menggambarkan alur bisnis konseptual, bukan spesifikasi teknis.

## 8. Penyesuaian ruang lingkup MVP dan kriteria penerimaan

Usulan kemampuan MVP yang diperjelas:
- Validasi kapasitas ketika pengajuan booking diterima dan sebelum admin menyetujui booking.
- Pencatatan status Visit secara terpisah dari Booking dan Service Execution.
- Pencatatan status setiap Service Execution secara terpisah.
- Pencatatan identitas, peran, dan kanal/sumber pembuat booking.
- Pemisahan status Booking, Visit, Service Execution, Invoice, dan Payment.

Kriteria penerimaan tambahan:
1. Pengajuan owner disimpan dengan status `MENUNGGU_KONFIRMASI`.
2. Sistem memvalidasi ulang kapasitas sebelum approval.
3. Approval ditolak jika kapasitas tidak lagi tersedia; booking tidak berubah menjadi `TERKONFIRMASI`.
4. Identitas admin dan waktu keputusan booking dapat ditelusuri sesuai refinement sebelumnya.
5. Status Visit dapat dilihat tanpa menimpa status Booking atau Service Execution.
6. Setiap Service Execution memiliki status tersendiri.
7. Perubahan status satu Service Execution tidak mengubah Visit atau Service Execution lainnya.
8. Status Payment dan Invoice tidak mengubah status Booking atau Visit.
9. Identitas, peran, dan kanal/sumber pembuat booking tersimpan dan dapat ditelusuri.

## 9. Keputusan yang masih terbuka

Sebelum BRD memperoleh sign-off stakeholder, hal berikut perlu divalidasi:
1. Apakah booking pending menahan kapasitas secara persisten.
2. Durasi hold, expiry, dan SLA approval jika slot hold digunakan.
3. Penanganan pengajuan ketika kapasitas tidak lagi tersedia pada saat approval.
4. Kebijakan booking yang dibuat admin atas nama pemilik.
5. Kapan Visit dibuat dalam proses check-in/kunjungan.
6. Apakah satu Visit dapat mencakup beberapa Service Execution dan bagaimana relasinya.
7. Nama status dan transisi final Visit serta Service Execution.
8. Peran yang berwenang memperbarui setiap status Visit dan Service Execution.
9. Kebijakan provider gateway, metode pembayaran, refund, retry, dan rekonsiliasi yang telah dicatat pada refinement sebelumnya.

## 10. Keterlacakan ke dokumen lain

| Kebutuhan bisnis | Artefak terkait |
|---|---|
| BR-57 — validasi ulang kapasitas sebelum approval | PR-14, PR-15, PR-59; SR-F-091 |
| BR-58 — status Visit terpisah | SR-F-101 |
| BR-59 — status Service Execution terpisah | SR-F-102 |
| BR-60 — identitas/peran/kanal pembuat booking | SR-F-103 |
| BRule-17 — pemisahan lifecycle | PR-45; SR-F-100 sampai SR-F-102 |

PRD Refinement v1.1 telah mencakup validasi kapasitas booking dan pemisahan Booking/Invoice/Payment. Penyelarasan product requirement untuk Visit, Service Execution, dan metadata sumber booking masih perlu dilakukan pada tahap refinement PRD berikutnya. Addendum ini tidak membuat ID PRD baru secara sepihak.

## 11. Status dokumen

BRD v1.0 tetap dipertahankan sebagai baseline. BRD Refinement v1.1 tetap tersedia sebagai riwayat refinement sebelumnya. Addendum v1.2 memperjelas validasi kapasitas, kebijakan pending slot yang masih terbuka, pemisahan Visit/Service Execution, lifecycle, dan traceability sumber booking.

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation. Dokumen ini belum merupakan persetujuan resmi dari pemilik atau manajemen klinik.

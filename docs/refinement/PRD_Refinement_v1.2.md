# RabbitCare — PRD Refinement Addendum v1.2

**Tanggal refinement:** 23 November 2026  
**Dokumen sumber:** PRD RabbitCare v1.0 dan PRD Refinement Addendum v1.1  
**Dokumen penyelarasan:** BRD Refinement v1.2, MRD Refinement v1.2, SRD Refinement v1.2, dan Gap Analysis Refinement v1.1  
**Status:** Proposed refinement / Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum lanjutan; tidak menggantikan dokumen v1.0 atau v1.1

## Ringkasan refinement

PRD Refinement v1.1 sudah mendefinisikan pengajuan booking oleh Owner, keputusan admin, validasi ulang kapasitas, pembayaran gateway, dan verifikasi hasil transaksi. Addendum v1.2 **tidak mengulang atau mengganti keseluruhan kebutuhan tersebut**. Addendum ini menutup tiga gap traceability yang ditemukan setelah penyelarasan BRD/MRD dan SRD:

1. **PR-66 — Visit lifecycle:** status kehadiran/progres kunjungan terpisah dari Booking, Service Execution, Invoice, dan Payment.
2. **PR-67 — Service Execution lifecycle:** status setiap layanan yang dilakukan dalam satu Visit dicatat dan ditampilkan secara terpisah.
3. **PR-68 — Booking provenance:** sistem mencatat identitas aktor, peran, dan kanal/sumber pembuat booking agar pengajuan Owner tidak tercampur dengan booking yang dibuat Admin.

Addendum ini juga menegaskan bahwa validasi ulang kapasitas saat approval merupakan kebutuhan yang telah dicakup PR-14 dan PR-59; kebijakan *persistent slot hold* selama `MENUNGGU_KONFIRMASI` **belum diputuskan**.

> **Batasan:** semua status dan aturan transisi Visit/Service Execution di bawah adalah proposed modeling values. Dokumen ini bukan bukti sign-off stakeholder maupun implementasi fitur.

## 1. Tujuan dan cakupan perubahan

- PRD v1.0 tetap menjadi baseline original.
- PRD Refinement v1.1 tetap menjadi addendum sebelumnya, termasuk PR-56 sampai PR-65.
- Untuk definisi lifecycle, metadata booking source, dan status pending-slot yang dijelaskan di sini, gunakan PRD Refinement v1.2 sebagai **usulan terbaru**.
- Semua requirement di luar perubahan eksplisit addendum ini tetap mengikuti PRD v1.0 dan PRD Refinement v1.1.

## 2. Ringkasan perubahan terhadap PRD Refinement v1.1

| Area | Kondisi v1.1 | Klarifikasi v1.2 |
|---|---|---|
| Product States | Status `CHECK_IN`, `MENUNGGU_ANTREAN`, `SEDANG_DILAYANI`, `SELESAI_DILAYANI`, dan `TIDAK_HADIR` masih dicantumkan pada Booking lifecycle. | Status operasional kunjungan dipindahkan ke Visit. Pelaksanaan satu layanan ditangani melalui Service Execution yang terpisah. |
| Booking approval | Owner mengajukan; Admin meninjau dan approve/reject dengan capacity revalidation. | PR-14 dan PR-59 dipertahankan. Tidak dibuat requirement baru yang menduplikasi capacity revalidation. |
| Pending slot | v1.1 mengusulkan bahwa pending request tidak mengunci slot secara permanen. | Usulan tersebut belum menjadi kebijakan final. Slot hold, durasi, expiry, dan SLA tetap membutuhkan keputusan stakeholder. |
| Traceability booking | Pencatatan keputusan Admin sudah disyaratkan. | Sumber dan aktor yang **membuat** booking juga dapat ditelusuri, termasuk Owner Portal versus Admin Console. |
| Billing and Payment | Invoice dan Payment Attempt dipisahkan dari Booking. | Pemisahan diperluas sehingga Payment/Invoice tidak otomatis mengubah status Visit atau Service Execution. |

## 3. Product lifecycle model

| Lifecycle | Proposed statuses | Tujuan pencatatan |
|---|---|---|
| **Booking** | `MENUNGGU_KONFIRMASI`, `TERKONFIRMASI`, `DITOLAK`, `DIBATALKAN`, `DIJADWALKAN_ULANG` | Pengajuan, keputusan, dan pengelolaan jadwal booking. |
| **Visit** | `BELUM_CHECK_IN`, `CHECKED_IN`, `MENUNGGU_ANTREAN`, `BERLANGSUNG`, `SELESAI`, `TIDAK_HADIR` | Kehadiran dan progres kunjungan pasien di klinik. |
| **Service Execution** | `BELUM_DIMULAI`, `SEDANG_DILAKSANAKAN`, `SELESAI` | Progres setiap layanan yang dicatat dalam satu Visit. |
| **Invoice** | `BELUM_DIBAYAR`, `LUNAS` | Status tagihan, terpisah dari operasional klinik. |
| **Payment Attempt** | `PENDING`, `PAID`, `FAILED`, `CANCELLED`, `REFUNDED` | Status satu percobaan transaksi gateway. |

**Aturan pemisahan lifecycle:**

- Status check-in, antrean, dan no-show tidak disimpan sebagai status Booking; status tersebut berada pada Visit.
- Progres pelaksanaan layanan tidak disimpan sebagai status Booking ataupun Visit; masing-masing Service Execution memiliki status sendiri.
- Perubahan status Visit tidak otomatis mengubah Booking, Service Execution, Invoice, atau Payment.
- Perubahan status satu Service Execution tidak otomatis mengubah status Visit maupun layanan lain.
- Payment hanya dapat menjadi `PAID` setelah hasil provider diverifikasi. Invoice menjadi `LUNAS` setelah pembayaran penuh terverifikasi; hal ini tidak mengubah status Booking atau Visit.
- `REFUNDED` dipertahankan sebagai status yang dikenal di baseline, tetapi refund otomatis bukan capability MVP yang telah disetujui.

**Asumsi model yang belum disetujui:** satu Booking menghasilkan paling banyak satu Visit pada model sementara. Satu Visit dapat mencatat beberapa Service Execution. Kebutuhan follow-up, kunjungan ulang, dan cardinality final memerlukan validasi stakeholder.

## 4. Product requirements tambahan

Requirement PR-01 sampai PR-65 tetap mengikuti PRD baseline v1.0 dan Refinement v1.1, kecuali klarifikasi lifecycle yang dicatat di bagian 3 addendum ini.

| ID | Product Requirement | Acceptance Criteria |
|---|---|---|
| **PR-66** | Sistem harus menampilkan dan melacak status Visit secara terpisah dari Booking, Service Execution, Invoice, dan Payment. | Setiap Visit memiliki identitas dan status tersendiri yang terhubung ke pasien/booking. Perubahan status Visit tidak menimpa status Booking, Service Execution, Invoice, atau Payment. |
| **PR-67** | Sistem harus menampilkan dan melacak status tiap Service Execution secara terpisah dari Visit dan dari layanan lainnya. | Setiap pelaksanaan layanan terhubung ke Visit dan jenis layanan, memiliki status dan waktu mulai/selesai bila relevan. Perubahan satu layanan tidak menimpa Visit atau layanan lainnya. |
| **PR-68** | Sistem harus mencatat identitas, peran, dan kanal/sumber aktor pembuat booking untuk membedakan pengajuan Owner dan booking yang dibuat Admin. | Setiap booking menyimpan metadata pembuat dari konteks terautentikasi di server, termasuk identitas, snapshot peran, dan sumber/kanal. Data ini dapat dilihat oleh peran berwenang untuk audit, dan tidak dapat dipalsukan lewat field client. |

**Batas keputusan PR-68:** pencatatan asal booking tidak menetapkan apakah booking yang dibuat Admin harus memperoleh approval. Aturan tersebut tetap open decision.

### 4.1 Ketentuan booking yang dipertahankan

- PR-13: pemilik mengirim pengajuan booking dengan status awal `MENUNGGU_KONFIRMASI`.
- PR-14: sistem memvalidasi kapasitas untuk mencegah overbooking.
- PR-15 / PR-56: admin berwenang dapat approve/reject pengajuan owner.
- PR-59: sistem wajib memvalidasi ulang kapasitas sebelum approval disimpan.
- Permintaan pending bukan bukti booking telah terkonfirmasi.
- **Persistent slot hold** pada status pending, durasinya, dan expiry belum ditetapkan sebagai kebijakan final. Penguncian sumber daya selama transaksi teknis tidak sama dengan penahanan slot sepanjang periode menunggu keputusan admin.

### 4.2 Ketentuan payment yang dipertahankan

- PR-60 sampai PR-65 tetap berlaku terkait gateway payment, referensi payment attempt, verifikasi server-side, receipt, monitoring, dan idempotency.
- Payment attempt tidak boleh menjadi `PAID` hanya karena pemilik kembali dari browser redirect.
- Ketidakpastian hasil pembuatan checkout/gateway timeout dan kebijakan retry perlu mengikuti SRD Refinement v1.2 serta keputusan provider/stakeholder.
- Payment/Invoice tidak mengubah Booking, Visit, atau Service Execution tanpa business rule eksplisit yang disetujui.

## 5. Core user journeys dan pengalaman produk

### 5.1 Booking request dan sumber pengajuan

1. Pemilik memilih kelinci, layanan, tanggal, dan slot yang ditampilkan sistem.
2. Sistem memvalidasi ketersediaan sebelum menyimpan pengajuan.
3. Permintaan disimpan dengan kode booking dan status `MENUNGGU_KONFIRMASI`.
4. Sistem menyimpan identitas, peran, dan kanal/sumber pembuat booking berdasarkan konteks autentikasi server.
5. Admin meninjau permintaan melalui review queue dan memeriksa jadwal/capacity.
6. Sistem memvalidasi ulang kapasitas ketika admin melakukan approval; approval gagal jika kapasitas tidak lagi tersedia.
7. Admin dapat menyetujui atau menolak pengajuan sesuai hak akses. Jika ditolak, alasan wajib dicatat.
8. Pemilik melihat keputusan booking melalui Owner Portal.

*Catatan:* kanal pembuatan booking dapat membedakan Owner Portal dan Admin Console. Langkah 1–8 secara khusus menggambarkan pengajuan Owner; approval untuk booking yang dibuat Admin belum diputuskan.

### 5.2 Visit dan Service Execution

1. Setelah pasien datang, petugas berwenang mencatat kehadiran dan progres Visit sesuai alur klinik.
2. Dalam Visit, petugas melihat daftar layanan yang dicatat untuk kunjungan tersebut.
3. Status setiap Service Execution diperbarui secara independen oleh petugas berwenang.
4. Status Visit menggambarkan progres kunjungan, bukan mengganti status tiap layanan.
5. Catatan klinis tetap ditelusuri ke identitas kelinci dan konteks Visit sesuai kebutuhan rekam medis.
6. Rincian invoice dapat diturunkan dari layanan dan item yang dicatat, sesuai mapping billing yang disepakati.

**Open decision:** kapan Visit dibuat, siapa yang dapat mengubah status, dan apakah satu Booking dapat menghasilkan lebih dari satu Visit harus dikonfirmasi sebelum sign-off.

### 5.3 Product experience implications

| Tampilan/area produk | Kebutuhan perilaku |
|---|---|
| Owner Portal — Booking | Menampilkan status pengajuan/keputusan booking; status Visit atau Service Execution tidak diberi label sebagai status Booking. |
| Clinic Portal — Booking review | Menampilkan sumber pembuat booking, informasi jadwal, kapasitas, dan keputusan admin sesuai role. |
| Clinic Portal — Visit | Menampilkan identitas pasien, booking terkait, check-in, antrean, serta status Visit. |
| Clinic Portal — Service Execution | Menampilkan layanan per Visit, status masing-masing, serta timestamp yang relevan. |
| Billing / Payment | Menampilkan Invoice dan Payment Attempt terpisah; receipt muncul hanya setelah hasil provider terverifikasi. |

Ini merupakan kebutuhan perilaku produk, **bukan klaim bahwa tampilan UI telah dibuat atau fitur sudah diimplementasikan**.

## 6. Product analytics dan evaluasi

Event booking, approval, dan pembayaran pada PRD Refinement v1.1 tetap dipertahankan (`booking_created`, `booking_approved`, `booking_rejected`, `payment_initiated`, `payment_status_updated`, `payment_completed`, dan `payment_failed`).

Indikator observasi tambahan yang disarankan saat pilot:

- Kelengkapan metadata identitas, peran, dan sumber pembuatan booking.
- Jumlah approval yang gagal karena kapasitas tidak tersedia saat keputusan admin.
- Proporsi Visit yang dapat ditelusuri ke booking dan profil kelinci terkait.
- Proporsi Service Execution dengan status yang tercatat secara mandiri.
- Jumlah ketidaksesuaian lifecycle, misalnya perubahan pembayaran yang tidak sengaja mengubah status Visit.

Tidak ada target numerik baru. Indikator di atas adalah usulan pengukuran pilot, bukan hasil pengukuran yang sudah dicapai.

## 7. Prioritas MVP, release alignment, dan acceptance

PR-66, PR-67, dan PR-68 **diusulkan sebagai Must Have**, konsisten dengan prioritas usulan BRD/MRD v1.2. Prioritas final memerlukan validasi stakeholder.

Penempatan pada release plan mengikuti pembagian PRD Refinement v1.1:
- **Release 2 — Scheduling and Booking:** penelusuran aktor/sumber booking (PR-68) dan capacity validation yang telah tercakup dalam PR-59.
- **Release 3 — Service Records:** pemisahan Visit dan Service Execution (PR-66, PR-67).
- **Release 4 — Billing and Reminder:** gateway payment dan invoice tetap mengikuti PR-60 sampai PR-65.

### 7.1 Acceptance criteria tambahan

Refinement v1.2 dapat diterima dalam pengujian jika:

1. Admin dapat melihat status Booking tanpa menganggap check-in atau pelaksanaan layanan sebagai status Booking.
2. Visit memiliki status tersendiri dan tidak berubah saat hanya status Booking, Service Execution, Invoice, atau Payment yang diperbarui.
3. Setiap Service Execution tercatat dalam Visit yang benar dengan status dan timestamp independen.
4. Pembaruan satu Service Execution tidak mengubah status Visit atau layanan lain secara tidak sengaja.
5. Payment/Invoice tidak mengubah status Booking, Visit, atau Service Execution secara otomatis.
6. Setiap booking menyimpan identitas, peran, dan kanal/sumber pembuat yang berasal dari autentikasi server.
7. Owner tidak dapat memalsukan `created_by_user_id`, `created_by_role`, atau `creation_channel` sebagai metadata otoritatif dari request client.
8. Admin berwenang dapat membedakan pengajuan Owner Portal dari booking yang dibuat Admin berdasarkan metadata sumber.
9. Capacity revalidation saat approval tetap diterapkan sebagaimana PR-59 dan tidak menghasilkan booking terkonfirmasi melebihi kapasitas.
10. Keputusan operasional yang masih terbuka tidak diperlakukan sebagai aturan final sebelum validasi stakeholder.

Acceptance criteria PRD v1.0 dan Refinement v1.1 lainnya tetap berlaku.

## 8. Traceability lintas dokumen

| Business / Market Need | Product Requirement | System Requirement |
|---|---|---|
| BR-57; MR-21 — capacity revalidation sebelum approval | PR-14, PR-15, PR-59 (v1.1) | SR-F-091 |
| BR-58; MR-24 — Visit terpisah dari Booking dan layanan | PR-66 (v1.2) | SR-F-101 |
| BR-59; MR-24 — Service Execution terpisah per layanan | PR-67 (v1.2) | SR-F-102 |
| BR-60; MR-25 — identitas/peran/kanal sumber booking | PR-68 (v1.2) | SR-F-103 |
| BRule-17 — lifecycle independen | PR-45 (v1.1), PR-66, PR-67 | SR-F-100 sampai SR-F-102 |

Gap Analysis Refinement v1.1 dan diagram PlantUML v1.2 tetap menjadi artefak analisis pendukung. Penyelarasan Gap Analysis versi berikutnya dapat dilakukan terpisah; addendum ini **tidak menautkan file Gap Analysis v1.2 yang belum ditambahkan pada tahap ini**.

## 9. Open decisions sebelum sign-off

- Apakah pengajuan booking `MENUNGGU_KONFIRMASI` menahan slot secara persisten; durasi hold dan expiry jika diterapkan.
- SLA admin serta penanganan pengajuan ketika kapasitas tidak lagi tersedia pada saat approval.
- Apakah booking yang dibuat Admin atas nama Owner memerlukan approval atau dapat langsung dikonfirmasi sesuai kewenangan.
- Kapan Visit dibuat, dan apakah satu Booking dapat menghasilkan lebih dari satu Visit.
- Nama status dan transisi final Visit serta Service Execution, termasuk kondisi no-show dan pembatalan satu layanan.
- Peran yang berwenang mengubah status Visit dan status Service Execution.
- Informasi Visit/Service Execution yang layak ditampilkan kepada Owner, jika diperlukan.
- Mapping rinci Service Execution dan item tercatat terhadap InvoiceItem.
- Provider gateway, retry/active payment attempt policy, refund, serta mekanisme rekonsiliasi.

## 10. Status dokumen

PRD v1.0 tetap menjadi baseline dan PRD Refinement v1.1 tetap tersedia sebagai riwayat refinement sebelumnya. Addendum ini hanya memperjelas pemisahan lifecycle, sumber booking, traceability, dan batas keputusan pending-slot.

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation. Dokumen ini tidak menyatakan bahwa requirement, aturan operasional, UI, atau fitur telah disetujui atau diimplementasikan.

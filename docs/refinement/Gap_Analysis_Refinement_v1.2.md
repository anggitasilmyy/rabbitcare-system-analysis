# RabbitCare — Gap Analysis Refinement Addendum v1.2

**Tanggal refinement:** 23 November 2026  
**Dokumen sumber:** RabbitCare Gap Analysis v1.0 dan Gap Analysis Refinement v1.1  
**Dokumen penyelarasan:** BRD/MRD Refinement v1.2, PRD Refinement v1.2, SRD Refinement v1.2 — Revised, dan PlantUML Refinement v1.2  
**Status:** Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum terarah; tidak menggantikan dokumen baseline v1.0 atau addendum v1.1

## Ringkasan refinement

Addendum v1.2 memperjelas dua area analisis yang perlu diselaraskan dengan refinement bisnis, pasar, produk, dan sistem terbaru:

1. **Pemisahan lifecycle operasional dan transaksi:** Booking, Visit, Service Execution, Invoice, dan Payment Attempt harus diperlakukan sebagai lifecycle berbeda yang saling terhubung, bukan satu status gabungan.
2. **Booking provenance:** sistem perlu menelusuri identitas, peran, dan kanal/sumber aktor yang membuat booking agar pengajuan Owner melalui Owner Portal dapat dibedakan dari booking yang dibuat Admin.

Addendum ini tidak mengulang seluruh analisis As-Is, SWOT, Fishbone, roadmap, atau 18 area proses pada v1.1. Bagian yang tidak secara eksplisit diperbarui tetap mengikuti Gap Analysis baseline v1.0 dan Gap Analysis Refinement v1.1.

> **Batasan analisis:** kondisi As-Is pada dokumen awal merupakan asumsi analisis yang masih perlu dikonfirmasi kepada stakeholder klinik. Istilah keputusan produk yang telah dicatat pada refinement sebelumnya menunjukkan arah kerja proyek, bukan bukti persetujuan formal stakeholder atau implementasi sistem.

## 1. Tujuan dan cakupan

Tujuan addendum ini adalah memperjelas dampak lifecycle separation dan booking provenance terhadap analisis kesenjangan RabbitCare.

Prinsip refinement:

- Gap Analysis v1.0 tetap menjadi baseline dan tidak diubah.
- Gap Analysis Refinement v1.1 tetap menjadi addendum sebelumnya, termasuk analisis 18 area proses, SWOT, Fishbone, prioritas akar masalah, roadmap, gap closure plan, dan indikator keberhasilan.
- Addendum v1.2 hanya memperbarui interpretasi target-state dan gap yang terkait dengan Visit, Service Execution, pemisahan lifecycle, dan asal pembuatan booking.
- Status dan transisi Visit serta Service Execution di bawah merupakan usulan model yang masih memerlukan validasi stakeholder.
- Addendum ini tidak menyatakan bahwa fitur telah dibangun, diuji, atau digunakan oleh klinik.

## 2. Pemisahan lifecycle

| Lifecycle | Proposed statuses | Fokus pencatatan |
|---|---|---|
| **Booking** | `MENUNGGU_KONFIRMASI`, `TERKONFIRMASI`, `DITOLAK`, `DIBATALKAN`, `DIJADWALKAN_ULANG` | Pengajuan booking, keputusan Admin, dan perubahan jadwal. |
| **Visit** | `BELUM_CHECK_IN`, `CHECKED_IN`, `MENUNGGU_ANTREAN`, `BERLANGSUNG`, `SELESAI`, `TIDAK_HADIR` | Kehadiran pasien dan progres encounter/kunjungan di klinik. |
| **Service Execution** | `BELUM_DIMULAI`, `SEDANG_DILAKSANAKAN`, `SELESAI` | Progres setiap layanan yang dicatat dalam satu Visit. |
| **Invoice** | `BELUM_DIBAYAR`, `LUNAS` | Nilai tagihan dan status pelunasan invoice. |
| **Payment Attempt** | `PENDING`, `PAID`, `FAILED`, `CANCELLED`, `REFUNDED` | Hasil setiap percobaan transaksi pembayaran. |

**Catatan status:** nama status Visit dan Service Execution merupakan proposed modeling values, bukan status final yang telah disetujui klinik. `REFUNDED` tetap dikenali sebagai status pada model pembayaran, tetapi kebijakan dan kemampuan refund bukan otomatis bagian dari MVP yang telah disetujui.

### 2.1 Aturan pemisahan lifecycle

- Booking merepresentasikan permintaan dan keputusan jadwal. Pengajuan Owner tetap berstatus `MENUNGGU_KONFIRMASI` sampai Admin berwenang menyetujui atau menolaknya.
- Visit merepresentasikan kehadiran dan progres kunjungan pasien. Status check-in, antrean, dan no-show tidak digunakan sebagai status Booking.
- Service Execution merepresentasikan pelaksanaan setiap layanan. Perubahan status satu layanan tidak boleh secara tidak sengaja menimpa status Visit atau layanan lainnya.
- Invoice merepresentasikan tagihan. Payment Attempt merepresentasikan satu percobaan pembayaran gateway.
- Pembayaran yang berhasil hanya dapat ditetapkan setelah hasil provider diverifikasi. Pembayaran terverifikasi dapat memperbarui Payment Attempt dan pelunasan Invoice, tetapi tidak otomatis mengubah status Booking, Visit, atau Service Execution.
- Riwayat medis tetap dapat ditelusuri melalui identitas kelinci/pasien (`Pet ID`) sesuai baseline. Visit dapat menjadi konteks encounter tambahan dan tidak menggantikan hubungan langsung pasien dengan rekam medis.

## 3. Refined To-Be clarification

Target-state yang diperjelas adalah sebagai berikut:

1. Owner mengajukan booking melalui Owner Portal. Pengajuan disimpan dengan status `MENUNGGU_KONFIRMASI` dan belum dianggap sebagai appointment yang terkonfirmasi.
2. Admin berwenang meninjau permintaan, memeriksa jadwal dan kapasitas, lalu menyetujui atau menolak pengajuan. Kapasitas harus divalidasi ulang sebelum approval disimpan sesuai kebutuhan yang telah tercantum pada refinement sebelumnya.
3. Sistem mencatat identitas aktor pembuat booking, snapshot peran, dan kanal/sumber pembuatan berdasarkan konteks autentikasi server. Metadata ini membedakan, misalnya, pengajuan melalui Owner Portal dari booking yang dibuat melalui Admin Console.
4. Ketika pasien datang, petugas berwenang mencatat kehadiran dan progres kunjungan pada Visit yang terpisah dari Booking.
5. Setiap layanan yang dicatat dalam Visit memiliki Service Execution tersendiri agar progres tiap layanan dapat ditelusuri tanpa menggabungkan semua status.
6. Catatan medis tetap terhubung dengan Pet ID dan dapat menggunakan Visit sebagai konteks encounter jika sesuai dengan rancangan data yang disetujui.
7. Invoice disusun dari layanan dan item yang tercatat. Hubungan rinci antara Service Execution, item layanan, dan InvoiceItem tetap memerlukan keputusan desain/billing.
8. Setiap pembayaran gateway dicatat sebagai Payment Attempt. Hasil sukses hanya memperbarui status pembayaran dan invoice setelah diverifikasi di sisi server.

**Batas keputusan:** pencatatan sumber booking tidak dengan sendirinya menetapkan apakah booking yang dibuat Admin harus melalui approval. Kebijakan tersebut tetap memerlukan keputusan stakeholder.

## 4. Gap dan dampak yang diperjelas

| Area analisis | Gap yang diperjelas pada v1.2 | Kapabilitas target yang diusulkan | Dampak yang diharapkan |
|---|---|---|---|
| **Visit tracking** | Check-in, antrean, dan progres kunjungan berpotensi tercampur dengan status Booking atau layanan. | Memiliki record dan lifecycle Visit yang terpisah. | Kehadiran dan progres kunjungan lebih mudah dipantau. |
| **Service tracking** | Status tiap layanan dalam satu kunjungan belum tentu dapat ditelusuri secara independen. | Membuat Service Execution untuk setiap layanan yang dicatat dalam Visit. | Staf dapat memantau progres layanan tanpa menimpa status Visit atau layanan lain. |
| **Lifecycle boundary** | Booking, Visit, Service Execution, Invoice, dan Payment dapat dipahami sebagai satu status proses. | Memisahkan record/status dan menghubungkannya melalui referensi data yang jelas. | Akuntabilitas, audit trail, dan pelaporan operasional lebih konsisten. |
| **Booking provenance** | Sumber dan aktor pembuat booking belum cukup jelas untuk membedakan pengajuan Owner dengan booking yang dibuat Admin. | Mencatat identitas aktor, snapshot peran, serta kanal/sumber dari konteks autentikasi server. | Asal pengajuan dan tanggung jawab pembuatan booking lebih mudah ditelusuri. |
| **Medical and billing traceability** | Pemisahan Visit berisiko membuat hubungan rekam medis dan rincian tagihan menjadi ambigu jika referensi data tidak dijaga. | Mempertahankan Pet ID sebagai penghubung rekam medis dan menentukan referensi Visit serta sumber item invoice secara eksplisit. | Kontinuitas riwayat pasien dan penelusuran tagihan lebih terjaga. |

## 5. Implikasi terhadap root-cause analysis

Pemisahan lifecycle memperjelas bahwa masalah analisis tidak hanya berkaitan dengan aktivitas manual, tetapi juga dengan batas tanggung jawab dan kejelasan status antarproses.

- **People:** admin dan staf memerlukan kejelasan tanggung jawab dalam membuat booking, mengambil keputusan, memperbarui Visit, dan memperbarui Service Execution.
- **Process:** proses harus membedakan pengajuan/approval booking, kehadiran pasien, pelaksanaan tiap layanan, dan penyelesaian pembayaran.
- **Technology:** aplikasi perlu mendukung record dan status yang terpisah, otorisasi perubahan, serta referensi antarentitas.
- **Data:** identitas pembuat booking, Pet ID, Visit, Service Execution, Invoice, dan Payment Attempt harus dapat ditelusuri tanpa menggabungkan lifecycle.
- **Management and Measurement:** indikator booking decision, no-show, penyelesaian Visit, progres layanan, dan rekonsiliasi pembayaran perlu didefinisikan secara terpisah.
- **Environment and External Services:** ketergantungan pada gateway, notifikasi, internet, dan hosting tetap mengikuti risiko yang dicatat pada Gap Analysis v1.1.

Diagram Fishbone v1.2 pada `plantuml/refinement/10_fishbone_gap_analysis_v1.2.puml` menjadi artefak pendukung untuk membaca akar masalah tersebut. Diagram dan addendum ini tetap merupakan analisis usulan, bukan hasil audit operasional klinik.

## 6. Items requiring stakeholder validation

Hal-hal berikut belum boleh dianggap sebagai kebijakan final:

1. Apakah satu Booking dapat menghasilkan lebih dari satu Visit, misalnya untuk kunjungan ulang atau follow-up.
2. Apakah satu Visit dapat berisi beberapa Service Execution dan bagaimana layanan tersebut dikelompokkan.
3. Nama status, transisi, kondisi no-show, pembatalan Visit, pembatalan layanan, dan partial service completion.
4. Peran yang berwenang membuat, melihat, dan memperbarui status Booking, Visit, serta Service Execution.
5. Apakah booking yang dibuat Admin atas nama Owner memerlukan approval atau dapat langsung dikonfirmasi sesuai kewenangan.
6. Nilai kanal/sumber booking yang akan digunakan serta informasi metadata yang perlu disimpan dan ditampilkan untuk audit.
7. Hubungan Service Execution dan item layanan dengan InvoiceItem, termasuk aturan layanan yang dapat ditagihkan.
8. Informasi Visit dan Service Execution yang perlu ditampilkan kepada Owner, jika diperlukan.
9. Indikator operasional dan target pengukuran untuk booking decision, Visit, service completion, no-show, dan pembayaran.

## 7. Traceability to related refinement

| Kebutuhan / artefak | ID terkait | Hubungan dengan Gap Analysis v1.2 |
|---|---|---|
| Business requirements | **BR-58, BR-59, BR-60** | Pemantauan Visit, Service Execution, dan traceability pembuat/sumber booking. |
| Market requirements | **MR-24, MR-25** | Kebutuhan pasar untuk lifecycle kunjungan/layanan dan booking provenance. |
| Product requirements | **PR-66, PR-67, PR-68** | Lifecycle Visit, lifecycle Service Execution, dan booking provenance. |
| System requirements | **SR-F-101, SR-F-102, SR-F-103** | Status Visit dan Service Execution terpisah serta metadata pembuat booking dari konteks autentikasi. |
| PlantUML v1.2 | Class Diagram, Visit State, Service Execution State, dan diagram booking. | Model pendukung untuk lifecycle separation dan booking provenance. |

## 8. Status dan preservasi versi

Addendum ini berstatus **Draft — pending stakeholder validation**. Dokumen ini memperjelas analisis target-state dan traceability, tetapi tidak membuktikan bahwa proses As-Is telah diverifikasi, stakeholder telah memberikan sign-off, atau kapabilitas telah diimplementasikan.

- Gap Analysis v1.0 tetap menjadi baseline.
- Gap Analysis Refinement v1.1 tetap tersedia dan tidak ditimpa.
- Gap Analysis Refinement v1.2 hanya memperbarui area lifecycle separation dan booking provenance.
- Analisis lain yang tidak secara eksplisit diubah tetap mengikuti baseline v1.0 dan addendum v1.1.

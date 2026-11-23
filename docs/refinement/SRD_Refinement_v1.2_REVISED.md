# RabbitCare — SRD Refinement Addendum v1.2

**Tanggal refinement:** 21 November 2026  
**Tanggal pemeriksaan/revisi:** 22 November 2026  
**Dokumen sumber:** RabbitCare System Requirements Document v1.0  
**Baseline:** Version 1.0 · Draft · Web-based client-server · Relational database  
**Status addendum:** Proposed refinement / Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum teknis; dokumen baseline dan diagram asli tetap dipertahankan

## Ringkasan keputusan dan tujuan refinement

Addendum ini menerjemahkan keputusan produk dan kebutuhan To-Be yang telah ditetapkan ke kebutuhan teknis RabbitCare:

1. **Booking yang diajukan pemilik memerlukan keputusan admin.** Owner mengirim pengajuan booking melalui Owner Portal. Pengajuan disimpan dengan status `MENUNGGU_KONFIRMASI` dan belum dianggap sebagai booking terkonfirmasi. Admin klinik meninjau lalu menyetujui atau menolak pengajuan tersebut. Status `TERKONFIRMASI` hanya diberikan setelah admin menyetujui dan sistem berhasil memvalidasi ulang kapasitas slot. Sistem juga merekam identitas aktor dan kanal/sumber saat booking dibuat untuk menjaga traceability. Pencatatan ini tidak menetapkan aturan approval untuk booking yang dibuat Admin atas nama pemilik; kebijakan tersebut tetap menjadi open decision.
2. **Payment gateway termasuk MVP.** Pemilik memulai pembayaran atas invoice yang telah diterbitkan melalui gateway yang dikonfigurasi. Sistem membuat payment attempt dan memverifikasi hasil transaksi dari provider. Status payment `PAID`, invoice `LUNAS`, dan receipt hanya boleh ditetapkan setelah hasil transaksi terverifikasi. Jika pembuatan checkout timeout atau hasilnya belum pasti, attempt tetap `PENDING` dan harus diperiksa melalui status inquiry jika tersedia atau exception handling.
3. **Status Visit dan pelaksanaan layanan dipisahkan.** Status kunjungan pasien tidak digabung dengan status pelaksanaan masing-masing layanan. Riwayat medis tetap dapat ditelusuri melalui identitas pasien (`Pet ID`) sesuai baseline `SR-F-009`. Nama status dan transisi Visit/Service Execution pada diagram v1.2 merupakan usulan pemodelan yang masih memerlukan validasi stakeholder.

SRD v1.0 tetap menjadi baseline. Requirement baseline `SR-F-001` sampai `SR-F-084` dan `SR-NF-001` sampai `SR-NF-034` dipertahankan. Addendum ini memperjelas requirement yang terdampak dan menambahkan `SR-F-085` sampai `SR-F-103` serta `SR-NF-035` sampai `SR-NF-039`. Requirement `SR-F-101` sampai `SR-F-102` menegaskan pemisahan status Visit dan pelaksanaan layanan sesuai target To-Be. Requirement `SR-F-103` menegaskan pencatatan identitas aktor dan kanal pembuat booking tanpa menetapkan kebijakan proses untuk booking yang dibuat Admin. ID pasien dan nomor rekam medis unik tetap mengacu pada `SR-F-009` baseline; tidak dibuat requirement duplikat. Pemetaan juga mencakup BR-53 sampai BR-56, MR-21 sampai MR-23, dan PR-56 sampai PR-65 dari dokumen refinement terkait.

**Dokumen sumber:** [RabbitCare System Requirements Document v1.0](../requirements/RabbitCare-SRD.pdf)

## 1. Ruang lingkup dan prinsip refinement

Refinement mencakup:
- Pengajuan booking oleh pemilik, admin approval/rejection, status transition, validasi kapasitas slot, authorization, audit, dan notification.
- Payment attempt untuk invoice yang telah diterbitkan melalui gateway, callback/webhook verification, payment status, invoice status, receipt, idempotency, dan monitoring.
- API requirements, data requirements, transaction rules, integration, logging, security, testing, traceability, dan system acceptance.
- Pembaruan diagram aktivitas, sequence, state, class, component, dan deployment dalam folder `plantuml/refinement/`, termasuk pemisahan state Visit dan Service Execution.

Batas cakupan:
- Aturan approval pada addendum ini berlaku untuk booking yang diajukan pemilik.
- Booking yang dibuat admin atas nama pemilik belum memiliki keputusan proses yang final.
- Waktu penerbitan invoice tetap mengikuti proses baseline BRD/PRD; addendum ini tidak menetapkan deposit atau pembayaran sebelum layanan.

Prinsip preservasi:
- Dokumen PDF SRD v1.0 tidak ditimpa.
- Diagram PlantUML asli di folder `plantuml/` tidak dihapus atau diubah oleh addendum ini.
- Requirement lama tetap dapat ditelusuri. Requirement baru menggunakan ID setelah nomor terakhir baseline.
- Hal yang belum diputuskan secara produk/operasional ditandai sebagai open decision, bukan ditetapkan diam-diam oleh desain teknis.
- Keputusan pada BRD, MRD, dan PRD refinement digunakan sebagai acuan konsistensi; detail yang masih berstatus proposed/open tidak dinaikkan menjadi aturan final di SRD.

## 2. Temuan baseline dan perubahan utama

| Area SRD v1.0 | Baseline | Refinement v1.2 |
|---|---|---|
| Section 3–5: System Context / Logical Architecture | Booking Service, Billing Service, Payment Channel, Notification Provider. | Perjelas alur Admin Review pada Booking Service dan Payment Gateway Adapter/Callback Handler pada Billing & Payment. |
| SR-F-019–027 | Sistem menerima booking, mengunci slot, menyimpan status awal, mengirim event konfirmasi, cancel/reschedule. | Untuk pengajuan owner, status awal menjadi `MENUNGGU_KONFIRMASI`; admin approval/rejection; slot divalidasi ulang saat approval. Aturan booking yang dibuat admin tetap open decision. |
| API 7.3 Schedule and Booking | `POST /api/bookings`, `GET /api/bookings/{booking_id}`, `PATCH .../confirm`, cancel, reschedule. | Pertahankan endpoint confirm sebagai admin approval; tambahkan reject dan daftar booking pending. |
| SR-F-058–066 | Invoice, payment record, metode pembayaran, status invoice/booking, receipt, double-payment prevention. | Payment attempt gateway untuk invoice yang telah diterbitkan, provider reference, callback verification, idempotency, receipt setelah verified paid; status booking tidak diubah oleh payment. |
| API 7.8 Billing and Payment | `POST /api/payments`, GET payment, receipt. | Pertahankan API dasar; tambahkan gateway callback/webhook dan kontrak status payment. |
| Data Requirements | Invoice 1-to-many Payment; Pet 1-to-many MedicalRecord. | Tegaskan satu record Payment mewakili satu payment attempt; tambahkan Payment Callback Event; modelkan Visit dan Service Execution sebagai lifecycle terpisah; pertahankan traceability MedicalRecord melalui Pet ID. |
| Authorization Matrix | Admin CRUD Payment; Owner own read. | Owner memulai/melihat pembayaran miliknya; Admin monitoring dan exception handling, bukan mengubah status gateway menjadi paid secara manual. |
| Transaction Rules | Admin/payment channel memproses pembayaran, lalu update payment, invoice, booking, receipt. | Pisahkan transaksi database internal dari proses eksternal gateway; verified callback memicu payment/invoice update; tidak mengubah booking lifecycle. |
| Integration Requirements | Payment Channel mendukung cash, transfer, QR, card, other; gateway callback disebut secara umum. | Gateway menjadi jalur pembayaran MVP; provider-specific signature, reference, amount, currency, event deduplication, dan retry diturunkan. |
| SR-NF-016–018 | Atomic booking/payment, duplicate prevention, notification retry. | Tegaskan atomicity database, idempotency callback, duplicate event handling, dan secure secret handling. |
| Visit / Service lifecycle | Status proses kunjungan dan pelaksanaan layanan belum dipisahkan eksplisit dalam refinement diagram. | Pisahkan status Visit dan status tiap Service Execution; nama state dan transisi ditandai proposed sampai divalidasi stakeholder. |

## 3. Logical architecture refinement

### 3.1 Booking components

**Booking Service**
- Menerima pengajuan booking yang dikirim Owner melalui Owner Portal.
- Memvalidasi token, role Owner, dan kepemilikan pet.
- Merekam identitas aktor pembuat booking (`created_by_user_id`), snapshot peran saat pembuatan (`created_by_role`), dan kanal/sumber pembuatan (`creation_channel`) berdasarkan konteks autentikasi server. Metadata ini digunakan untuk traceability dan tidak menentukan kebijakan approval booking yang dibuat Admin.
- Meminta Schedule Service memeriksa slot.
- Membuat pengajuan booking Owner dengan status `MENUNGGU_KONFIRMASI`.
- Menyediakan review queue untuk admin.
- Memproses approval/rejection hanya untuk pengajuan yang masih menunggu keputusan, dengan pemeriksaan otorisasi dan audit.
- Memvalidasi ulang kapasitas slot dalam transaksi saat admin menyetujui.
- Memisahkan pengajuan Owner dari booking yang dibuat Admin atas nama pemilik; aturan booking yang dibuat Admin menunggu keputusan stakeholder.

**Schedule Service**
- Menyediakan slot berdasarkan jadwal petugas dan kapasitas.
- Mengunci data jadwal/kapasitas selama transaksi pembuatan request atau approval.
- Memastikan tidak ada booking terkonfirmasi melebihi kapasitas slot.
- Mengembalikan `BOOKING_SLOT_UNAVAILABLE` bila kapasitas sudah tidak tersedia saat approval.

**Notification Service**
- Menerima event `booking_submitted`, `booking_approved`, dan `booking_rejected`.
- Mengirim notifikasi ke pemilik dan/atau admin sesuai kanal yang tersedia.
- Mencatat delivery status mengikuti SR-F-067 sampai SR-F-073 dan requirement notifikasi baseline.

### 3.2 Visit and Service Execution data responsibilities

**Visit**
- Mewakili satu kunjungan pasien yang terkait dengan booking terkonfirmasi.
- Menyimpan status kehadiran dan progres kunjungan, bukan status setiap layanan.
- Terhubung dengan Pet dan Booking agar riwayat kunjungan dapat ditelusuri.

**Service Execution (PelaksanaanLayanan)**
- Mewakili pelaksanaan satu layanan yang dicatat dalam suatu Visit.
- Menyimpan status pelaksanaan dan timestamp mulai/selesai secara terpisah dari Visit.
- Perubahan status pelaksanaan layanan tidak mengubah status Visit, Booking, Invoice, atau Payment.

Nama status dan transisi pada diagram v1.2 merupakan proposed modeling values; keputusan operasional final tetap memerlukan validasi stakeholder.

### 3.3 Billing and Payment components

**Billing Service**
- Mengelola invoice dan item invoice.
- Mengambil layanan dan item yang telah dicatat sebagai dasar penyusunan baris invoice.
- Menentukan nilai invoice yang masih harus dibayar.
- Menghubungkan invoice dengan satu atau lebih payment attempt.
- Menandai invoice `LUNAS` hanya setelah Payment Service menyatakan transaksi berhasil terverifikasi.

**Payment Service**
- Membuat payment attempt untuk invoice.
- Menghasilkan reference/idempotency key internal yang unik.
- Menyimpan provider transaction reference setelah diterima dari provider; nilainya dapat null selama inisiasi belum terkonfirmasi, beserta status payment attempt.
- Mengelola status `PENDING`, `PAID`, `FAILED`, `CANCELLED`, dan `REFUNDED`.
- Memastikan satu invoice tidak memperoleh dua pembayaran sukses yang tidak disengaja.

**Payment Gateway Adapter**
- Mengisolasi format API provider dari domain RabbitCare.
- Memulai transaksi/checkout melalui provider yang dipilih.
- Menerima callback/webhook provider melalui endpoint khusus.
- Memvalidasi signature atau mekanisme autentikasi callback yang didukung provider.
- Memverifikasi kecocokan reference, invoice, amount, currency, dan status transaksi.
- Mengirim hasil terverifikasi ke Payment Service.

**Receipt Service**
- Menghasilkan receipt setelah status payment `PAID` terverifikasi.
- Menghubungkan receipt dengan invoice dan payment reference.
- Tidak menghasilkan receipt lunas untuk transaksi pending, failed, atau callback yang tidak valid.

Provider gateway belum dipilih. Nama endpoint provider, algoritma signature, format callback, dan metode verifikasi final harus mengikuti dokumentasi provider setelah pemilihan dilakukan.

## 4. Booking workflow dan status

### 4.1 Alur teknis pengajuan oleh Owner

1. Owner Portal mengirim `POST /api/bookings`.
2. API Gateway memvalidasi access token dan meneruskan request.
3. Booking Service memvalidasi role Owner, kepemilikan pet, layanan, jadwal, dan data wajib.
4. Schedule Service memeriksa ketersediaan kapasitas.
5. Dalam transaksi database, sistem mengunci record jadwal/kapasitas selama proses insert, membuat booking ID dan booking code, menyimpan pengajuan dengan status `MENUNGGU_KONFIRMASI`, serta merekam `created_by_user_id`, `created_by_role`, dan `creation_channel` dari konteks autentikasi server. Untuk alur ini, nilai sumber menunjukkan Owner dan Owner Portal.
6. Setelah commit, sistem menerbitkan event `booking_submitted` untuk notifikasi bahwa permintaan telah diterima, bukan bahwa booking telah terkonfirmasi.
7. Pengajuan muncul di review queue admin.

Alur ini berlaku untuk booking yang diajukan Owner. Pembuatan booking oleh Admin atas nama Owner tidak dicakup oleh aturan status pada bagian ini sampai kebijakannya ditetapkan stakeholder.

### 4.2 Alur keputusan admin

Aturan berikut berlaku untuk pengajuan booking yang dibuat oleh Owner dan berstatus `MENUNGGU_KONFIRMASI`.

**Approval**
- Hanya Admin berwenang yang dapat menyetujui.
- Sistem memvalidasi bahwa status booking masih `MENUNGGU_KONFIRMASI` dan belum memiliki keputusan sebelumnya.
- Sistem mengunci jadwal/kapasitas dan memeriksa ketersediaan kembali di dalam transaksi.
- Jika kapasitas tersedia, sistem mengubah status menjadi `TERKONFIRMASI`, menyimpan `decided_by_user_id`, `decision_at`, dan audit log.
- Jika slot tidak lagi tersedia, sistem menolak aksi approval dengan `BOOKING_SLOT_UNAVAILABLE`; booking tidak boleh berubah menjadi `TERKONFIRMASI`.
- Jika booking sudah diputuskan, sistem menolak keputusan ulang dengan `BOOKING_ALREADY_DECIDED`.
- Setelah commit, sistem menerbitkan `booking_approved`.

**Rejection**
- Hanya Admin berwenang yang dapat menolak.
- `rejection_reason` wajib diisi.
- Sistem mengubah status menjadi `DITOLAK`, menyimpan `decided_by_user_id`, `decision_at`, alasan, dan audit log.
- Jika booking sudah diputuskan, sistem menolak keputusan ulang dengan `BOOKING_ALREADY_DECIDED`.
- Setelah commit, sistem menerbitkan `booking_rejected`.

**Transisi keputusan booking pada addendum ini:**

| Status awal | Aksi | Status akhir | Ketentuan |
|---|---|---|---|
| `MENUNGGU_KONFIRMASI` | Admin approve | `TERKONFIRMASI` | Hanya jika kapasitas slot tersedia saat divalidasi ulang. |
| `MENUNGGU_KONFIRMASI` | Admin reject | `DITOLAK` | `rejection_reason` wajib diisi. |
| `TERKONFIRMASI` atau `DITOLAK` | Admin approve/reject ulang | Tidak berubah | Aksi ditolak dengan `BOOKING_ALREADY_DECIDED`. |

Transisi lain seperti cancellation, reschedule, check-in, dan layanan tetap mengikuti lifecycle pada SRD v1.0 dan tidak diubah oleh tabel ini.

### 4.3 Slot lock dan pending policy

SR-F-021 baseline mewajibkan penguncian slot selama proses pembuatan booking. Refinement mempertahankan **transactional lock** pada proses request dan approval untuk mencegah race condition. Transactional lock adalah penguncian record jadwal/kapasitas di database hanya selama transaksi berjalan dan dilepas ketika transaksi commit atau rollback. Lock ini merupakan kontrol konkurensi, bukan reservasi kapasitas yang terus berlaku setelah transaksi request selesai.

Hal yang berbeda adalah **persistent slot hold**, yaitu penahanan kapasitas setelah pengajuan `MENUNGGU_KONFIRMASI` selesai disimpan sampai booking diputuskan atau kedaluwarsa. BRD refinement mencatat usulan kontrol tersebut, tetapi MRD dan PRD tetap menandai kebijakan hold/expiry sebagai hal yang memerlukan validasi stakeholder. Karena itu, persistent slot hold belum diperlakukan sebagai aturan final oleh addendum SRD ini.

**Keputusan yang belum ditetapkan:**
- Apakah pengajuan `MENUNGGU_KONFIRMASI` langsung menahan kapasitas setelah transaksi request selesai.
- Berapa lama hold berlaku dan kapan booking kedaluwarsa.
- Kapan kapasitas dilepaskan setelah rejection, cancellation, atau expiry.
- Bagaimana perlakuan booking yang dibuat Admin atas nama Owner.

SRD ini tidak menetapkan durasi hold atau expiry secara sepihak. Implementasi final mengikuti keputusan stakeholder. Dalam semua kondisi, sistem wajib mengunci data kapasitas selama transaksi yang relevan, memvalidasi ulang slot saat approval, dan mencegah booking terkonfirmasi melebihi kapasitas.

## 5. Payment gateway workflow

### 5.1 Initiation dan payment attempt

1. Setelah layanan dicatat dan invoice diterbitkan sesuai proses baseline BRD/PRD, Owner membuka invoice yang belum lunas.
2. Owner memulai pembayaran melalui gateway.
3. Billing Service memvalidasi bahwa invoice tersedia, terkait dengan Owner tersebut, belum `LUNAS`, dan memiliki outstanding amount yang dapat dibayar.
4. Nilai amount dan currency diambil dari data invoice/konfigurasi sistem; klien tidak boleh menentukan atau mengubah nominal pembayaran.
5. Payment Service membuat payment attempt baru dengan status `PENDING`, internal reference, idempotency key, amount, currency, dan invoice ID.
6. Gateway Adapter meminta provider membuat transaksi/checkout.
7. Jika provider mengembalikan transaction reference dan instruksi/URL checkout, sistem menyimpannya dan mengembalikan instruksi tersebut kepada Owner. Jika reference belum diterima karena timeout atau respons tidak pasti, field reference tetap null dan attempt ditangani sesuai exception flow.
8. Owner menyelesaikan pembayaran pada alur provider.

Addendum ini tidak menambahkan pembayaran deposit atau pembayaran sebelum layanan. Invoice creation timing tetap mengikuti baseline dan keputusan produk yang berlaku.

### 5.2 Callback verification

Callback/webhook tidak boleh dipercaya hanya karena request diterima atau browser diarahkan kembali ke RabbitCare. Redirect browser bukan bukti pembayaran berhasil.

Sebelum callback dapat mengubah status payment atau invoice, sistem harus:
- Memvalidasi signature atau mekanisme autentikasi/integritas callback sesuai spesifikasi provider.
- Memeriksa provider transaction reference dan internal payment reference.
- Memastikan invoice yang dirujuk sesuai dengan payment attempt.
- Memastikan amount dan currency sesuai dengan nilai invoice/payment attempt yang tersimpan di server.
- Memeriksa status transaksi dan memastikan jenis event termasuk event yang didukung.
- Memeriksa provider event reference atau mekanisme deduplication yang didukung provider agar callback yang sama tidak diproses dua kali.
- Mencatat waktu penerimaan, hasil verifikasi, dan processing status tanpa menyimpan secret atau data pembayaran sensitif.

Event yang belum terverifikasi boleh dicatat untuk audit/monitoring dengan status `UNVERIFIED`, tetapi tidak boleh memicu perubahan status payment/invoice, penerbitan receipt, atau notifikasi pembayaran berhasil. Callback dengan signature tidak valid atau data mismatch harus ditandai untuk exception handling. Callback dengan signature valid tetapi jenis event tidak dikenal/tidak didukung harus dicatat sebagai `UNSUPPORTED`; callback yang tidak dapat dikorelasikan dengan payment attempt harus dicatat sebagai `NEEDS_REVIEW`. Kedua kondisi tersebut tidak boleh mengubah status Payment/Invoice. Jika provider tidak memberikan event reference atau internal payment reference yang dapat dicocokkan, event tetap dicatat tanpa `payment_id` sampai dapat dikorelasikan.

Jika provider menyediakan server-to-server status inquiry, mekanisme tersebut dapat digunakan sebagai verifikasi tambahan sesuai dokumentasi provider. Pilihan final menunggu provider ditentukan.

### 5.3 Status update dan receipt

- Callback valid dengan hasil berhasil: payment attempt menjadi `PAID`; invoice menjadi `LUNAS` setelah sistem memastikan belum ada attempt lain yang lebih dahulu menyelesaikan invoice; receipt diterbitkan setelah perubahan database berhasil commit.
- Callback valid dengan hasil gagal: payment attempt menjadi `FAILED`; invoice tetap `BELUM_DIBAYAR`.
- Callback valid dengan hasil dibatalkan: payment attempt menjadi `CANCELLED`; invoice tetap `BELUM_DIBAYAR`.
- Callback dengan hasil pending: payment attempt tetap `PENDING`; tidak ada receipt lunas.
- Callback tidak valid atau data mismatch: status payment tidak berubah menjadi `PAID`; event dicatat untuk monitoring/exception handling.
- Callback/event berulang yang sudah diproses: hasil diperlakukan idempotent dan tidak membuat update invoice atau receipt ganda.
- Jika provider melaporkan lebih dari satu attempt berhasil untuk invoice yang sama, sistem harus menandai kondisi tersebut sebagai payment exception untuk rekonsiliasi. Sistem tidak boleh melakukan refund otomatis atau mengubah catatan transaksi provider tanpa kebijakan yang disetujui.

**Transisi status payment attempt:**

| Status awal | Hasil provider terverifikasi | Status akhir | Ketentuan |
|---|---|---|---|
| `PENDING` | Berhasil | `PAID` | Hanya setelah verifikasi berhasil dan validasi invoice/reference lolos. |
| `PENDING` | Gagal | `FAILED` | Invoice tetap `BELUM_DIBAYAR`. |
| `PENDING` | Dibatalkan | `CANCELLED` | Invoice tetap `BELUM_DIBAYAR`. |
| `PENDING` | Masih diproses | `PENDING` | Tidak ada receipt lunas. |
| `PAID` | Refund disetujui dan diproses | `REFUNDED` | Hanya jika fitur/prosedur refund disetujui; bukan bagian MVP saat ini. |

Callback terlambat atau hasil provider yang bertentangan dengan status attempt yang sudah terminal harus masuk exception handling dan diverifikasi melalui mekanisme provider. Sistem tidak boleh melakukan transisi status yang tidak sah hanya berdasarkan urutan kedatangan callback.

**Pemetaan status invoice:**

| Kondisi | Status Payment Attempt | Status Invoice |
|---|---|---|
| Invoice diterbitkan dan belum ada pembayaran berhasil | Belum ada attempt atau `PENDING` | `BELUM_DIBAYAR` |
| Provider mengonfirmasi transaksi masih pending | `PENDING` | `BELUM_DIBAYAR` |
| Provider mengonfirmasi transaksi gagal | `FAILED` | `BELUM_DIBAYAR` |
| Provider mengonfirmasi transaksi dibatalkan | `CANCELLED` | `BELUM_DIBAYAR` |
| Satu attempt berhasil dan terverifikasi | `PAID` | `LUNAS` |
| Refund diproses | `REFUNDED` sesuai proses yang kelak disetujui | Pemetaan status invoice memerlukan keputusan tersendiri |

Status `REFUNDED` dipertahankan karena tercantum pada baseline, tetapi proses refund dan dampaknya terhadap status invoice belum menjadi bagian MVP.

**Pemisahan lifecycle:** pembayaran hanya memperbarui Payment dan Invoice. Status Booking maupun Visit tidak diperbarui oleh Payment Service. `BELUM_DIBAYAR`/`LUNAS` diperlakukan sebagai ringkasan status invoice, sedangkan booking mengikuti lifecycle operasionalnya sendiri.

### 5.4 Batas MVP dan open decisions

- Gateway adalah jalur pembayaran invoice dalam MVP sesuai keputusan produk.
- Provider dan metode pembayaran belum ditentukan.
- Pembayaran tunai/manual sebagai alternatif, partial payment, deposit, refund otomatis, dan multi-provider belum disetujui dalam scope ini.
- Status `REFUNDED` tetap dipertahankan sebagai status yang tercantum pada baseline, tetapi proses refund tidak otomatis menjadi fitur MVP.
- Retry payment attempt, settlement, reconciliation schedule, dan SLA callback harus disesuaikan setelah provider dipilih.

## 6. Functional requirements refinement

Requirement baseline `SR-F-001` sampai `SR-F-084` tetap berlaku. Requirement berikut ditambahkan:

Klarifikasi untuk `SR-F-058` dan `SR-F-059`: invoice tetap terkait dengan booking, sedangkan item invoice diturunkan dari layanan dan item yang benar-benar dicatat. Relasi sumber item perlu dapat ditelusuri; detail mapping/cardinality mengikuti detailed billing design.

| ID | System Requirement | Acceptance Criteria |
|---|---|---|
| SR-F-085 | Sistem harus menyimpan pengajuan booking yang dibuat Owner dengan status awal `MENUNGGU_KONFIRMASI`. | Booking ID/code terbentuk; status awal tersimpan; pengajuan Owner belum dianggap terkonfirmasi. |
| SR-F-086 | Sistem harus menyediakan daftar pengajuan booking Owner yang menunggu keputusan admin. | Admin berwenang dapat memfilter dan membuka pengajuan Owner berstatus `MENUNGGU_KONFIRMASI`. |
| SR-F-087 | Sistem harus mengizinkan Admin berwenang menyetujui atau menolak pengajuan booking Owner. | Approval/rejection hanya berlaku pada pengajuan Owner yang masih berstatus `MENUNGGU_KONFIRMASI`; keputusan ulang ditolak. |
| SR-F-088 | Sistem harus menyimpan identitas admin dan waktu keputusan booking. | `decided_by_user_id` dan `decision_at` tersimpan dan dapat diaudit. |
| SR-F-089 | Sistem harus mewajibkan alasan ketika admin menolak booking. | Rejection tanpa alasan ditolak dengan validation error. |
| SR-F-090 | Sistem harus menampilkan status dan hasil keputusan booking kepada pemilik. | Owner hanya melihat booking miliknya, status terbaru, dan alasan penolakan jika booking ditolak serta alasan tersedia. |
| SR-F-091 | Sistem harus memvalidasi ulang kapasitas slot saat admin menyetujui booking. | Approval gagal bila kapasitas sudah tidak tersedia; tidak ada overbooking terkonfirmasi. |
| SR-F-092 | Sistem harus mencatat dan menerbitkan event pengajuan, approval, dan rejection. | Event hanya diterbitkan setelah perubahan database berhasil commit. |
| SR-F-093 | Sistem harus membuat payment attempt melalui payment gateway untuk invoice yang telah diterbitkan dan belum lunas. | Payment attempt terhubung ke invoice yang benar, amount/currency dihitung dari data invoice di server, dan status awal `PENDING`. |
| SR-F-094 | Sistem harus menyimpan internal payment reference dan provider transaction reference. | Setiap attempt dapat ditelusuri ke invoice; provider transaction reference disimpan setelah diterima dari provider dan boleh bernilai null sebelum tersedia. |
| SR-F-095 | Sistem harus memverifikasi autentisitas dan konsistensi callback gateway sebelum memperbarui status pembayaran. | Signature/authentication, reference, invoice, amount, currency, jenis event, dan status diperiksa; callback tidak valid, tidak dikenal, tidak didukung, atau tidak dapat dikorelasikan tidak mengubah status payment/invoice dan dicatat untuk exception handling. |
| SR-F-096 | Sistem hanya boleh menetapkan payment `PAID` dan invoice `LUNAS` setelah transaksi terverifikasi berhasil. | Redirect browser atau callback tidak valid tidak dapat menandai invoice lunas. |
| SR-F-097 | Sistem harus menerbitkan receipt setelah payment `PAID` terverifikasi. | Receipt memiliki invoice ID dan payment reference; tidak dibuat untuk pending/failed/cancelled. |
| SR-F-098 | Sistem harus memproses callback gateway secara idempotent. | Callback/event yang sama tidak menggandakan payment, update invoice, receipt, atau efek notifikasi pembayaran berhasil. |
| SR-F-099 | Sistem harus menyediakan monitoring transaksi gateway untuk admin berwenang. | Admin dapat melihat status, invoice, reference, waktu, callback yang tidak terverifikasi/tidak dikenal/tidak dapat dikorelasikan, dan transaksi yang perlu tindak lanjut. |
| SR-F-100 | Sistem harus memisahkan lifecycle booking dari lifecycle invoice/payment. | Update payment tidak mengubah status operasional booking maupun Visit. |
| SR-F-101 | Sistem harus menyimpan status Visit secara terpisah dari status Booking, Service Execution, Invoice, dan Payment. | Status Visit dapat ditelusuri melalui Visit ID dan relasi Booking/Pet; perubahan status Visit tidak menimpa status Booking, Service Execution, Invoice, atau Payment. |
| SR-F-102 | Sistem harus mencatat status setiap pelaksanaan layanan secara terpisah dari status Visit, Booking, Invoice, dan Payment. | Setiap record PelaksanaanLayanan memiliki status dan timestamp pelaksanaan; perubahan status layanan tidak menimpa status Visit, Booking, Invoice, atau Payment. |
| SR-F-103 | Sistem harus merekam identitas aktor dan kanal/sumber ketika booking dibuat. | Setiap booking menyimpan `created_by_user_id`, `created_by_role`, dan `creation_channel` berdasarkan konteks autentikasi server; metadata membedakan booking yang diajukan Owner dari booking yang dibuat Admin tanpa menetapkan aturan approval untuk booking Admin. |

## 7. Nonfunctional requirements tambahan

Requirement `SR-NF-001` sampai `SR-NF-034` tetap berlaku. Tambahkan:

| ID | Requirement |
|---|---|
| SR-NF-035 | Callback gateway harus diverifikasi menggunakan mekanisme autentikasi/integritas yang didukung provider sebelum diproses. |
| SR-NF-036 | Pemrosesan callback dan update payment harus idempotent untuk mencegah transaksi ganda. |
| SR-NF-037 | Kredensial, secret, dan signing key gateway harus disimpan pada konfigurasi environment/secret store dan tidak ditanam pada source code. |
| SR-NF-038 | Log aplikasi tidak boleh menyimpan password, access token lengkap, signing secret, CVV, atau data pembayaran sensitif. |
| SR-NF-039 | Perubahan payment dan invoice di database harus konsisten secara transaksional; pemanggilan eksternal gateway dikelola sebagai proses terpisah dengan status dan retry/exception handling. Penerbitan receipt dan event setelah commit harus dapat diulang secara aman (idempotent) agar kegagalan proses setelah commit tidak menghilangkan hasil yang perlu disampaikan. |

Tidak ada target numerik performa, availability, atau SLA gateway baru yang ditetapkan oleh addendum ini.

## 8. API requirements refinement

API baseline dipertahankan kecuali penjelasan/penambahan berikut.

### 8.1 Schedule and Booking

| Method | Endpoint | Akses | Refinement |
|---|---|---|---|
| POST | `/api/bookings` | Owner | Membuat pengajuan yang dikirim Owner dengan status `MENUNGGU_KONFIRMASI`; tidak langsung terkonfirmasi. |
| GET | `/api/bookings/{booking_id}` | Owner pemilik booking; Admin/role sesuai authorization | Mengambil detail dan status booking. |
| GET | `/api/bookings?status=MENUNGGU_KONFIRMASI` | Admin | Daftar pengajuan untuk review queue. |
| PATCH | `/api/bookings/{booking_id}/confirm` | Admin | Endpoint baseline dipertahankan; bermakna approve setelah revalidation slot. |
| PATCH | `/api/bookings/{booking_id}/reject` | Admin | Endpoint baru; body memuat `rejection_reason`. |
| PATCH | `/api/bookings/{booking_id}/cancel` | Sesuai aturan baseline | Pembatalan mengikuti policy yang disepakati. |
| PATCH | `/api/bookings/{booking_id}/reschedule` | Sesuai aturan baseline | Reschedule mengikuti validasi jadwal dan status yang sah. |

Identitas pembuat booking dan kanal pembuatan ditentukan oleh konteks autentikasi serta kanal aplikasi di sisi server. Client tidak boleh mengirim `created_by_user_id` atau `created_by_role` sebagai nilai otoritatif.

### 8.2 Billing and Payment

| Method | Endpoint | Akses | Refinement |
|---|---|---|---|
| GET | `/api/invoices/by-booking/{booking_id}` | Owner terkait; Admin | Melihat invoice terkait booking. |
| POST | `/api/payments` | Owner terkait | Membuat payment attempt gateway untuk invoice yang telah diterbitkan dan belum lunas; amount/currency ditentukan server dari invoice. |
| GET | `/api/payments/{payment_id}` | Owner terkait; Admin | Melihat status payment attempt. |
| GET | `/api/invoices/{invoice_id}/receipt` | Owner terkait; Admin | Mengambil receipt setelah paid terverifikasi. |
| POST | `/api/payments/gateway/callback` | Payment provider | Endpoint callback/webhook; autentikasi melalui signature/provider mechanism, bukan access token Owner. |

### 8.3 Visit and Service Execution API considerations

Endpoint berikut merupakan usulan kontrak untuk mendukung requirement status Visit dan pelaksanaan layanan; akses final dan payload perlu diselaraskan dengan authorization matrix serta desain API:

| Method | Endpoint | Akses | Refinement |
|---|---|---|---|
| GET | `/api/visits/{visit_id}` | Role terkait sesuai authorization | Mengambil status Visit dan ringkasan kunjungan. |
| GET | `/api/visits/{visit_id}/service-executions` | Role terkait sesuai authorization | Mengambil daftar layanan yang dicatat dalam Visit beserta status masing-masing. |
| PATCH | `/api/service-executions/{service_execution_id}/status` | Petugas berwenang sesuai jenis layanan | Memperbarui status pelaksanaan satu layanan tanpa mengubah status Visit/Booking/Invoice/Payment. |

Endpoint di atas adalah proposed API mapping, bukan kontrak final provider atau implementasi yang sudah tersedia.

### 8.4 Contract dan response rules

Ketentuan kontrak tambahan:
- Request pembayaran harus mengidentifikasi invoice yang akan dibayar dan menggunakan mekanisme idempotency yang ditetapkan API.
- Client tidak boleh mengirim nominal pembayaran sebagai nilai otoritatif; server menghitung amount dan currency dari invoice.
- Akses invoice dan payment harus diverifikasi terhadap kepemilikan Owner atau role Admin yang berwenang.
- Callback provider menggunakan autentikasi provider-specific dan tidak menggunakan access token Owner.
- Format request/response final mengikuti standar API pada SRD v1.0 dan dokumentasi provider yang dipilih.
- Untuk endpoint `POST /api/payments`, client wajib mengirim `Idempotency-Key` yang unik untuk setiap aksi inisiasi pembayaran.
- Sistem menyimpan dan memvalidasi key tersebut dalam ruang lingkup Owner dan invoice terkait.
- Jika request dikirim ulang menggunakan key yang sama dengan invoice dan parameter material yang sama, sistem harus mengembalikan payment attempt yang sudah terbentuk tanpa membuat attempt baru.
- Jika key yang sama digunakan kembali dengan invoice atau parameter material yang berbeda, sistem harus menolak request dengan `IDEMPOTENCY_KEY_CONFLICT`.
- Untuk membuat payment attempt baru secara sengaja, client harus menggunakan key yang berbeda.
- `payment_id` dan `internal_reference` tetap dihasilkan oleh sistem.

**Contoh request rejection**
```json
{
  "rejection_reason": "Jadwal petugas tidak tersedia pada waktu tersebut."
}
```

**Contoh data internal payment attempt**
```json
{
  "invoice_id": "invoice-uuid",
  "payment_id": "payment-uuid",
  "internal_reference": "RC-PAY-REFERENCE",
  "provider_transaction_reference": null,
  "amount": 150000,
  "currency": "IDR",
  "status": "PENDING"
}
```

Field `provider_transaction_reference` dapat bernilai null ketika payment attempt internal baru dibuat dan belum menerima reference dari provider. Field tersebut diisi setelah provider mengembalikan reference transaksi. Field dan format final provider callback harus mengikuti provider yang dipilih. Contoh di atas adalah ilustrasi kontrak internal, bukan payload provider tertentu.

Standard API response pada Section 8 SRD v1.0 tetap digunakan. Error response mengikuti format baseline.

### 8.5 Error codes tambahan

| Error Code | Deskripsi |
|---|---|
| `BOOKING_APPROVAL_REQUIRED` | Booking belum disetujui admin sehingga proses lanjutan tidak dapat dilakukan. |
| `BOOKING_ALREADY_DECIDED` | Booking sudah diputuskan dan tidak dapat diputuskan ulang melalui aksi yang sama. |
| `BOOKING_REJECTION_REASON_REQUIRED` | Alasan rejection wajib diisi. |
| `BOOKING_SLOT_UNAVAILABLE` | Slot tidak tersedia saat approval divalidasi ulang. |
| `PAYMENT_GATEWAY_UNAVAILABLE` | Provider gateway tidak dapat diakses atau gagal membuat transaksi. |
| `PAYMENT_CALLBACK_INVALID` | Signature/autentikasi callback tidak valid. |
| `PAYMENT_REFERENCE_MISMATCH` | Reference transaksi tidak cocok dengan payment attempt. |
| `PAYMENT_AMOUNT_MISMATCH` | Amount atau currency tidak cocok dengan invoice/payment attempt. |
| `PAYMENT_STATUS_CONFLICT` | Transisi status payment tidak sah. |
| `PAYMENT_EVENT_DUPLICATE` | Callback/event telah diproses; sistem mengembalikan hasil idempotent sesuai kontrak provider/API dan tidak membuat efek ganda. |
| `IDEMPOTENCY_KEY_CONFLICT` | Idempotency key yang sama digunakan kembali dengan invoice atau parameter material yang berbeda. |

Error codes baseline seperti `SCHEDULE_NOT_AVAILABLE`, `BOOKING_INVALID_STATUS`, `INVOICE_ALREADY_PAID`, `PAYMENT_FAILED`, dan `VALIDATION_ERROR` tetap dipertahankan.

## 9. Data requirements dan validation

### 9.1 Booking data

Tambahan/penegasan atribut pada entitas Booking:
- `status`
- `requested_at`
- `created_by_user_id` (wajib; identitas aktor yang membuat booking)
- `created_by_role` (wajib; snapshot peran aktor saat booking dibuat)
- `creation_channel` (wajib; kanal/sumber pembuatan, misalnya `OWNER_PORTAL` atau `ADMIN_PORTAL`)
- `decided_by_user_id` (nullable sampai keputusan dibuat)
- `decision_at` (nullable sampai keputusan dibuat)
- `rejection_reason` (nullable; wajib jika status `DITOLAK`)

Atribut keputusan hanya diisi untuk pengajuan Owner yang melalui proses approval pada addendum ini. Metadata pembuat booking dicatat untuk semua booking yang dibuat melalui aplikasi; untuk pengajuan Owner, aktornya adalah Owner dan kanalnya `OWNER_PORTAL`, sedangkan untuk booking yang dibuat Admin, aktornya adalah Admin dan kanalnya dapat dicatat sebagai `ADMIN_PORTAL`. Pencatatan metadata ini tidak berarti booking Admin otomatis terkonfirmasi atau harus melalui approval; aturan statusnya tetap mengikuti keputusan stakeholder yang masih terbuka.

Relasi Booking ke Owner, Pet, Schedule, Visit, CheckIn, Queue, MedicalRecord, dan Invoice mengikuti baseline yang relevan serta refinement pada bagian ini.

### 9.2 Visit dan Service Execution data

Untuk mendukung pemisahan lifecycle pada kebutuhan To-Be, model data refinement mencakup:

**Visit**
- `visit_id`
- `booking_id`
- `pet_id`
- `visit_date`
- `status`
- `checked_in_at` (nullable)
- `closed_at` (nullable)

**PelaksanaanLayanan (Service Execution)**
- `service_execution_id`
- `visit_id`
- `service_id`
- `status`
- `started_at` (nullable)
- `completed_at` (nullable)
- `execution_notes` (nullable)

Satu Visit dapat memiliki nol atau lebih record PelaksanaanLayanan. Setiap record pelaksanaan merujuk pada satu layanan katalog. Status Visit merepresentasikan kehadiran/progres kunjungan; status PelaksanaanLayanan merepresentasikan progres satu layanan. Nama state dan transisi final memerlukan validasi stakeholder. Relasi satu Booking ke maksimal satu Visit pada model saat ini merupakan asumsi pemodelan sementara dan harus dikonfirmasi terhadap aturan operasional, termasuk kemungkinan kunjungan ulang atau follow-up.

Rekam medis tetap memiliki relasi langsung dengan Pet agar riwayat kesehatan dapat dicari berdasarkan Pet ID. Relasi ke Visit dapat digunakan untuk menelusuri konteks kunjungan yang menghasilkan catatan medis. Kebutuhan Pet ID dan nomor rekam medis unik sudah ditetapkan pada `SR-F-009` baseline dan tidak diduplikasi.

**Invoice item traceability:** ketika baris invoice berasal dari pelaksanaan layanan atau item yang telah dicatat, sistem menyimpan jenis dan referensi sumber (`source_type`, `source_reference_id`) agar baris tagihan dapat ditelusuri. Mapping dan cardinality rinci ditetapkan pada detailed billing design.

### 9.3 Payment data

Satu record `Payment` mewakili satu payment attempt. Satu Invoice dapat memiliki beberapa Payment attempt sampai satu attempt berhasil.

Atribut yang diperlukan:
- `payment_id`
- `invoice_id`
- `provider_code`
- `internal_reference`
- `provider_transaction_reference` (nullable sampai provider mengembalikan reference transaksi)
- `idempotency_key`
- `amount`
- `currency`
- `status`
- `initiated_at`
- `paid_at` (nullable)
- `failure_code` / `failure_message` (nullable)

Ketentuan data:
- `internal_reference` harus unik.
- `idempotency_key` harus unik dalam ruang lingkup yang ditetapkan sistem.
- Amount dan currency diambil dari invoice/konfigurasi sistem, bukan dipercaya dari input client.
- `paid_at` hanya diisi setelah hasil transaksi sukses terverifikasi.

### 9.4 Payment Callback Event

Tambahkan entitas teknis `PaymentCallbackEvent` untuk deduplication dan audit:
- `event_id` internal
- `provider_code`
- `provider_event_reference` (nullable; unik per provider jika tersedia)
- `payment_id` (nullable sampai event berhasil dikorelasikan dengan payment attempt)
- `event_type` (jenis event yang diterima, termasuk event yang belum dikenal)
- `received_at`
- `verification_status` (misalnya `UNVERIFIED`, `VERIFIED`, `INVALID`, atau `MISMATCH`)
- `processing_status` (misalnya `RECEIVED`, `PROCESSED`, `DUPLICATE`, `UNSUPPORTED`, atau `NEEDS_REVIEW`)
- `payload_hash` atau metadata non-sensitif yang diperlukan untuk audit

Event yang signature-nya valid tetapi memiliki jenis event yang tidak didukung dicatat dengan `verification_status=VERIFIED` dan `processing_status=UNSUPPORTED`; event yang tidak dapat dikorelasikan ke payment attempt dicatat dengan `payment_id` null dan `processing_status=NEEDS_REVIEW`. Keduanya tidak boleh mengubah status Payment/Invoice atau menerbitkan receipt sampai ditangani sesuai kontrak provider dan prosedur exception.

Jangan menyimpan signing secret, CVV, password, access token lengkap, atau raw sensitive payment credentials.

### 9.5 Relasi dan constraint

- `Invoice 1 — 0..* Payment` (setiap payment merupakan attempt).
- `Payment 1 — 0..* PaymentCallbackEvent` setelah event berhasil dikorelasikan; event yang belum dapat dikorelasikan boleh tersimpan sementara dengan `payment_id` null.
- `Booking 1 — 0..1 Visit` merupakan asumsi model refinement sementara; Visit terkait dengan satu Pet dan satu booking yang mendasarinya. Cardinality ini perlu divalidasi stakeholder sebelum technical sign-off.
- `Visit 1 — 0..* PelaksanaanLayanan`; setiap PelaksanaanLayanan merujuk pada satu `Layanan`.
- `Pet 1 — 0..* MedicalRecord` dipertahankan agar riwayat medis dapat ditelusuri langsung melalui Pet ID.
- `Booking 1 — 0..1 Invoice` tetap mengikuti baseline.
- Invoice dibuat berdasarkan booking serta layanan dan item yang telah dicatat; `InvoiceItem` dapat menyimpan referensi sumber untuk traceability. Mapping dan cardinality rinci antara pelaksanaan layanan dan baris invoice ditetapkan pada detailed billing design.
- Internal payment reference harus unik.
- Provider transaction reference harus unik dalam ruang lingkup provider bila provider menjamin keunikannya.
- Provider event reference harus unik dalam ruang lingkup provider bila tersedia; jika provider tidak menyediakan event reference, strategi deduplication alternatif harus ditetapkan setelah provider dipilih.
- Idempotency key harus memiliki constraint unik sesuai ruang lingkup yang ditetapkan sistem.
- Amount harus sesuai outstanding invoice; currency harus sesuai konfigurasi invoice/provider.
- Amount/currency yang dikirim client tidak menjadi sumber kebenaran.
- Hanya satu payment attempt yang boleh menyelesaikan invoice menjadi `LUNAS`.
- Repeated callback tidak boleh membuat receipt atau pembayaran sukses kedua.
- Receipt harus memiliki relasi unik ke payment attempt yang berhasil agar retry penerbitan tidak membuat receipt ganda.
- Jika terdapat lebih dari satu transaksi provider yang terverifikasi berhasil untuk invoice yang sama, kondisi tersebut harus ditandai sebagai exception dan ditangani melalui rekonsiliasi sesuai kebijakan yang disetujui.

## 10. Authorization matrix refinement

Hak akses lain tetap mengikuti Authorization Matrix pada Section 12 SRD v1.0. Area yang diperjelas:

| Fitur | Owner | Admin | Doctor | Groomer | Manager |
|---|---|---|---|---|---|
| Booking | Create / Read own | Read / Approve / Reject pengajuan Owner; create/manage lain sesuai baseline dan policy terpisah | Read | Read | Read |
| Payment | Initiate / Read own / receipt own | Read / Monitor / exception handling | No | No | Read |
| Invoice | Read own | CRUD sesuai baseline | Read sesuai kebutuhan layanan | Read sesuai kebutuhan layanan | Read |

Admin tidak boleh mengubah status transaksi gateway menjadi `PAID` hanya melalui input manual. Penetapan `PAID` berasal dari hasil provider yang terverifikasi. Jika pembayaran manual/tunai akan dipertahankan sebagai alternatif, alur dan kontrolnya memerlukan keputusan produk serta requirement terpisah.

## 11. Transaction rules refinement

### 11.1 Booking request transaction

1. Validasi token dan role Owner.
2. Validasi kepemilikan pet.
3. Validasi layanan, jadwal, dan kapasitas.
4. Lock record slot/kapasitas selama transaksi.
5. Insert pengajuan booking dengan status `MENUNGGU_KONFIRMASI` serta metadata `created_by_user_id`, `created_by_role`, dan `creation_channel` dari konteks autentikasi server.
6. Simpan audit/request metadata lain yang diwajibkan baseline.
7. Commit.
8. Publish `booking_submitted` setelah commit melalui mekanisme event yang dapat diulang secara aman.

Jika langkah validasi atau insert gagal, transaksi database dibatalkan. Notifikasi tidak dikirim sebagai konfirmasi booking terkonfirmasi; pesan kepada Owner hanya menyatakan bahwa permintaan telah diterima setelah commit.

### 11.2 Booking approval transaction

1. Validasi token dan role Admin.
2. Ambil pengajuan booking Owner dan pastikan status `MENUNGGU_KONFIRMASI` serta belum memiliki keputusan.
3. Lock jadwal/kapasitas dan validasi ulang slot.
4. Update status `TERKONFIRMASI`, `decided_by_user_id`, dan `decision_at`.
5. Insert audit log.
6. Commit.
7. Publish `booking_approved` melalui mekanisme event yang dapat diulang secara aman.

Jika slot tidak tersedia, transaksi approval dibatalkan dan booking tidak menjadi terkonfirmasi. Jika booking telah diputuskan sebelumnya, request ditolak dan tidak boleh menimpa keputusan terdahulu.

### 11.3 Booking rejection transaction

1. Validasi token dan role Admin.
2. Ambil pengajuan booking Owner dan pastikan status `MENUNGGU_KONFIRMASI` serta belum memiliki keputusan.
3. Validasi `rejection_reason`.
4. Update status `DITOLAK`, `decided_by_user_id`, `decision_at`, dan alasan.
5. Insert audit log.
6. Commit.
7. Publish `booking_rejected` melalui mekanisme event yang dapat diulang secara aman.

### 11.4 Payment transaction

**Internal initiation**
1. Validasi token/role, invoice, dan kepemilikan.
2. Pastikan invoice telah diterbitkan dan belum `LUNAS`.
3. Ambil amount dan currency dari invoice; jangan mempercayai nominal dari client.
4. Buat payment attempt `PENDING` dengan internal reference dan idempotency key.
5. Commit record internal.
6. Minta gateway membuat transaksi/checkout.
7. Simpan provider reference dan instruksi checkout. Jika provider secara definitif menolak inisiasi sebelum transaksi terbentuk, attempt dapat ditandai `FAILED` dengan `failure_code` yang sesuai; kondisi ini tidak menandai invoice lunas.

Jika request ke payment gateway mengalami timeout atau menghasilkan respons yang tidak dapat dipastikan, sistem harus mempertahankan payment attempt dalam status `PENDING`, membiarkan `provider_transaction_reference` bernilai null jika belum diterima, dan menandainya untuk pemeriksaan lebih lanjut. Sistem tidak boleh langsung membuat payment attempt baru sebelum memastikan apakah transaksi sebelumnya berhasil dibuat oleh provider. Jika tersedia, sistem menggunakan mekanisme server-to-server status inquiry untuk memverifikasi transaksi. Jika status belum dapat dipastikan, transaksi masuk ke exception handling untuk tindak lanjut. Jika provider memberikan penolakan definitif bahwa transaksi tidak dibuat, attempt dapat ditandai `FAILED` dengan `failure_code` seperti `GATEWAY_INITIATION_FAILED`. Kebijakan apakah Idempotency-Key baru boleh membuat attempt tambahan ketika attempt lain masih aktif belum ditetapkan dan tidak boleh diasumsikan oleh diagram sebagai aturan final.

**Verified callback processing**
1. Terima callback dan catat metadata penerimaan sebagai event `UNVERIFIED` bila diperlukan untuk audit.
2. Verifikasi signature/authentication sesuai dokumentasi provider; jika tidak valid, tandai event `INVALID`/`REJECTED` dan hentikan pemrosesan status.
3. Cocokkan payment reference, invoice, amount, currency, jenis event, dan status.
4. Jika jenis event tidak didukung atau payment attempt tidak dapat dikorelasikan, catat event sebagai `UNSUPPORTED` atau `NEEDS_REVIEW`, simpan tanpa payment link bila perlu, dan hentikan pemrosesan status.
5. Periksa deduplication key/provider event reference; event yang telah diproses ditangani secara idempotent.
6. Lock payment dan invoice terkait setelah payment attempt berhasil diidentifikasi.
7. Validasi transisi status dan pastikan invoice belum diselesaikan oleh attempt lain.
8. Update Payment dan Invoice secara atomik; callback invalid/mismatch tidak boleh mengubah keduanya menjadi paid.
9. Catat event processing/audit.
10. Commit.
11. Jika status `PAID`, terbitkan receipt dan kirim notifikasi setelah commit melalui mekanisme yang dapat diulang secara aman. Receipt harus memiliki constraint unik agar retry tidak menghasilkan receipt ganda.

Pemanggilan gateway eksternal tidak berada dalam satu transaksi ACID database yang sama. Konsistensi dijaga dengan payment state, idempotency, event processing, dan exception handling. Outbox atau mekanisme retryable equivalent dapat digunakan untuk memastikan event/receipt setelah commit tidak hilang saat terjadi kegagalan proses. Booking status tidak diperbarui pada transaksi payment.

## 12. Integration, logging, security, dan deployment

### 12.1 Payment gateway integration

- Gunakan adapter agar provider dapat diganti tanpa mengubah aturan domain Billing/Payment.
- Simpan konfigurasi endpoint, merchant identifier, dan secret pada environment/secret store.
- Gunakan HTTPS.
- Verifikasi callback sesuai dokumentasi provider sebelum mengizinkan perubahan status.
- Catat reference, event ID, waktu diterima, hasil verifikasi, dan hasil pemrosesan.
- Pisahkan event yang belum terverifikasi dari event yang telah lolos verifikasi.
- Jangan menyimpan raw payload yang memuat kredensial atau data pembayaran sensitif; gunakan payload hash atau metadata minimum yang diperlukan.
- Jangan mengasumsikan satu provider atau metode tertentu sebelum keputusan provider dibuat.

### 12.2 Logging and monitoring

Pertahankan logging baseline (request ID, user ID, endpoint, response status, process time, internal error, notification delivery, payment callback, scheduler execution). Tambahkan:
- booking request ID, creator identity/role/channel, booking decision actor/time, dan hasil slot revalidation;
- internal payment reference, provider reference, callback event reference, verification status, dan processing status;
- indikator transaksi pending/failed/mismatch yang membutuhkan tindak lanjut.

Jangan log password, full token, signing secret, CVV, atau informasi pembayaran sensitif.

### 12.3 Deployment

Komponen deployment baseline tetap berlaku. Tambahkan koneksi HTTPS dari Application Server/Payment Gateway Adapter ke Payment Provider. Callback provider diarahkan ke endpoint webhook yang tersedia di environment production/staging sesuai konfigurasi provider. Environment credentials harus dipisahkan untuk development, testing/staging, dan production.

## 13. Testing requirements tambahan

| Jenis test | Skenario |
|---|---|
| Unit | Booking status transition, rejection reason required, slot revalidation, payment state transition, invoice total/reference validation. |
| Authorization | Owner hanya melihat booking/payment miliknya; hanya Admin dapat approve/reject; role lain tidak dapat mengubah payment. |
| Concurrency | Dua request/approval mencoba memakai kapasitas slot yang sama; tidak boleh ada overbooking terkonfirmasi. |
| Integration - Booking | Owner request → pending → admin approve/reject → owner status; creator identity/channel tersimpan dari konteks autentikasi; notification event hanya setelah commit; booking yang dibuat Admin diuji setelah kebijakannya ditetapkan. |
| Integration - Payment | Create payment attempt, gateway success/pending/failed/cancelled, receipt after verified success. |
| Callback security | Invalid signature, invalid reference, amount mismatch, currency mismatch, callback replay, duplicate event, unsupported event type, unmatched callback/payment reference. |
| Idempotency | Callback sama dikirim berulang; hanya satu payment outcome dan satu receipt. |
| Failure handling | Gateway timeout/unavailable, definitive gateway initiation failure, callback terlambat, status inquiry mismatch, retry/exception monitoring. |
| Visit / Service Execution | Perubahan status Visit tidak mengubah status pelaksanaan layanan; perubahan status satu layanan tidak mengubah status Visit atau layanan lain. |
| Billing traceability | Invoice line item dapat ditelusuri ke layanan dan item tercatat yang menjadi sumbernya. |
| Regression | Check-in, queue, medical record, vaccination, grooming, reminder, reporting, audit log tetap berfungsi. |
| UAT | Owner, Admin, Doctor, Groomer, Manager sesuai peran dan acceptance criteria. |

## 14. Traceability matrix refinement

| Business / Market / Product Requirement | System Requirement |
|---|---|
| BR-53: Owner-submitted booking requires admin decision | SR-F-085–087 |
| BR-54: Payment gateway included in MVP | SR-F-093–098 |
| BR-55: Decision metadata and rejection reason are recorded | SR-F-088–089 |
| BR-56 / BRule-16: Duplicate payment callback must not create duplicate processing or receipt | SR-F-095, SR-F-097–098; SR-NF-036, SR-NF-039 |
| BRule-10: Invoice is paid only after verified gateway success | SR-F-095–097 |
| BRule-15: Owner-submitted booking requires admin approval/rejection | SR-F-085–092 |
| MR-21: Admin reviews, approves/rejects Owner booking and Owner sees result | SR-F-085–092 |
| MR-22: Owner pays invoice through gateway and sees verified status/receipt | SR-F-093–098 |
| MR-23: Clinic monitors gateway transactions and exceptions | SR-F-099 |
| PR-13: Owner submits booking | SR-F-019–024, SR-F-085 |
| PR-15: Admin decides Owner-submitted booking | SR-F-025, SR-F-087–092 |
| PR-43: Admin monitors gateway payment status and references | SR-F-095–096, SR-F-099 |
| PR-44: Receipt after verified payment | SR-F-097 |
| PR-45: Invoice is marked paid after verified success; booking lifecycle remains separate | SR-F-096, SR-F-100 |
| PR-56: Admin review queue and decision | SR-F-086–087 |
| PR-57: Decision metadata and rejection reason | SR-F-088–089 |
| PR-58: Owner sees booking status | SR-F-090 |
| PR-59: Prevent confirmed slot overcapacity | SR-F-021–022, SR-F-091 |
| PR-60: Owner initiates gateway payment | SR-F-058–063, SR-F-093 |
| PR-61: Payment reference per attempt | SR-F-063, SR-F-094 |
| PR-62: Verified payment status | SR-F-064, SR-F-095–096 |
| PR-63: Receipt after verified payment | SR-F-065, SR-F-097 |
| PR-64: Admin transaction monitoring | SR-F-099 |
| PR-65: Idempotent gateway processing | SR-F-066, SR-F-098 |
| Booking/payment lifecycle separation | SR-F-100 |
| To-Be: Visit dan service status tracked separately | SR-F-101–102 |
| Technical traceability: booking creator identity and creation channel | SR-F-103 |

Traceability requirement baseline BR-01 sampai BR-52 dan PR-01 sampai PR-55 tetap mengikuti section traceability pada SRD v1.0. Requirement yang diperjelas oleh addendum ini dirujuk secara eksplisit pada tabel di atas.

## 15. System acceptance criteria tambahan

Sistem memenuhi refinement ini apabila:
1. Owner dapat mengirim pengajuan booking dan status awal pengajuan tersebut `MENUNGGU_KONFIRMASI`.
2. Hanya Admin berwenang yang dapat approve/reject pengajuan booking Owner.
3. Rejection mewajibkan alasan; identitas admin dan waktu keputusan tercatat.
4. Keputusan booking hanya dapat dilakukan satu kali untuk pengajuan yang masih pending.
5. Approval memvalidasi ulang kapasitas slot dan tidak menghasilkan overbooking terkonfirmasi.
6. Owner dapat melihat status dan hasil keputusan booking miliknya.
7. Kebijakan slot hold/expiry mengikuti keputusan stakeholder dan tidak menggunakan durasi yang belum disetujui.
8. Payment attempt hanya dapat dibuat untuk invoice yang telah diterbitkan, belum lunas, dan dimiliki oleh Owner yang melakukan pembayaran.
9. Amount/currency payment ditentukan server dari invoice dan terhubung ke payment attempt yang benar.
10. Callback diverifikasi terhadap signature/authentication, reference, invoice, amount, currency, jenis event, dan status; event tidak dikenal/tidak didukung atau tidak dapat dikorelasikan dicatat untuk exception handling tanpa mengubah Payment/Invoice.
11. Hanya transaksi sukses terverifikasi yang mengubah Payment menjadi `PAID` dan Invoice menjadi `LUNAS`.
12. Receipt hanya diterbitkan setelah status `PAID` terverifikasi.
13. Callback duplikat/replay tidak menggandakan payment, invoice update, receipt, atau notifikasi sukses.
14. Admin dapat memonitor transaksi dan exception tanpa mengubah hasil provider secara manual.
15. Payment tidak mengubah booking lifecycle.
16. Seluruh requirement baseline yang tidak berubah tetap lulus regression test dan UAT.
17. Status Visit tersimpan terpisah dari status Booking, Service Execution, Invoice, dan Payment.
18. Status setiap Service Execution tersimpan terpisah dari status Visit dan layanan lainnya; riwayat medis tetap dapat ditelusuri melalui Pet ID.
19. Invoice line items dapat ditelusuri ke layanan dan item yang telah dicatat sesuai mapping billing yang disepakati.
20. Identitas aktor, snapshot peran, dan kanal/sumber pembuat setiap booking tercatat berdasarkan konteks autentikasi server dan tidak dapat ditentukan secara sepihak oleh client.

## 16. Open decisions sebelum technical sign-off

- Provider gateway, merchant account, metode pembayaran, currency, biaya, settlement, dan dokumentasi callback.
- Apakah server-to-server status inquiry tersedia/diwajibkan oleh provider.
- Kebijakan booking yang dibuat Admin atas nama Owner: apakah memerlukan approval, atau dapat langsung dikonfirmasi sesuai kewenangan.
- Kebijakan booking Owner berstatus pending: apakah langsung menahan slot, durasi hold, expiry, pelepasan slot setelah rejection/cancellation, dan SLA approval admin.
- Apakah pembayaran tunai/manual tersedia sebagai alternatif gateway.
- Retry payment attempt, rekonsiliasi, refund, deposit, partial payment, dan multi-provider.
- Penanganan jika lebih dari satu payment attempt untuk invoice yang sama terverifikasi berhasil, termasuk receipt, overpayment, dan refund.
- Strategi deduplication jika provider tidak menyediakan event reference yang unik.
- Aturan invoice expiry dan perubahan invoice setelah payment attempt dibuat.
- Notification channel/template untuk booking submitted/approved/rejected dan payment outcome.
- Target operasional callback processing dan gateway availability setelah provider dipilih.
- Status dan transisi final Visit serta Service Execution, termasuk aturan pembentukan Visit dan jumlah layanan yang dapat dicatat dalam satu Visit.

## 17. Diagram refinement

Diagram asli tetap berada di `plantuml/`. Versi refinement v1.1 dipertahankan. Diagram v1.2 ditambahkan sebagai versi baru di `plantuml/refinement/` agar riwayat perubahan dan baseline dapat ditelusuri. Pada audit diagram, Class Diagram perlu diperiksa terhadap metadata pembuat booking, nullable reference pada Payment, kemungkinan callback event yang belum terhubung ke Payment, serta cardinality Booking–Visit yang masih bersifat asumsi. Sequence/Activity Payment perlu memperlihatkan penanganan callback tidak dikenal/tidak terhubung tanpa perubahan status transaksi.

- [Use Case Context v1.2](../../plantuml/refinement/01_use_case_context_v1.2.puml)
- [Use Case Owner Portal v1.2](../../plantuml/refinement/02_use_case_owner_portal_v1.2.puml)
- [Use Case Clinic Operations v1.2](../../plantuml/refinement/03_use_case_clinic_operations_v1.2.puml)
- [Activity — Booking Service v1.2](../../plantuml/refinement/05_activity_booking_service_v1.2.puml)
- [Activity — Payment v1.2](../../plantuml/refinement/09_activity_payment_v1.2.puml)
- [Fishbone Analysis v1.2](../../plantuml/refinement/10_fishbone_gap_analysis_v1.2.puml)
- [Sequence — Booking v1.2](../../plantuml/refinement/11_sequence_booking_v1.2.puml)
- [Sequence — Payment Gateway v1.2](../../plantuml/refinement/15_sequence_payment_v1.2.puml)
- [Class Diagram v1.2](../../plantuml/refinement/17_class_diagram_v1.2.puml)
- [Booking State v1.2](../../plantuml/refinement/18_state_booking_v1.2.puml)
- [Component Diagram v1.2](../../plantuml/refinement/19_component_diagram_v1.2.puml)
- [Deployment Diagram v1.2](../../plantuml/refinement/20_deployment_diagram_v1.2.puml)
- [Payment State v1.2](../../plantuml/refinement/21_state_payment_v1.2.puml)
- [Visit State v1.2](../../plantuml/refinement/22_state_visit_v1.2.puml)
- [Service Execution State v1.2](../../plantuml/refinement/23_state_service_execution_v1.2.puml)

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation. Addendum ini tidak menetapkan provider gateway, booking policy untuk booking yang dibuat Admin, slot hold duration, SLA, metode pembayaran yang belum disetujui, maupun state final Visit/Service Execution.
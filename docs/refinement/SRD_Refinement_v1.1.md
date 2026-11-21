# RabbitCare — SRD Refinement Addendum v1.1

**Tanggal refinement:** 21 November 2026  
**Dokumen sumber:** RabbitCare System Requirements Document v1.0  
**Baseline:** Version 1.0 · Draft · Web-based client-server · Relational database  
**Status addendum:** Proposed refinement / Draft — pending stakeholder validation  
**Jenis perubahan:** Addendum teknis; dokumen baseline dan diagram asli tetap dipertahankan

## Ringkasan keputusan dan tujuan refinement

Addendum ini menerjemahkan dua keputusan produk yang telah ditetapkan ke kebutuhan teknis RabbitCare:

1. **Booking = admin approval.** Pemilik mengirim pengajuan booking. Sistem menyimpan pengajuan dengan status `MENUNGGU_KONFIRMASI`. Admin klinik menjadi pihak yang menyetujui atau menolak. Status `TERKONFIRMASI` hanya diberikan setelah admin menyetujui dan sistem berhasil memvalidasi ulang ketersediaan slot.
2. **Payment gateway termasuk MVP.** Pemilik memulai pembayaran invoice melalui gateway yang dikonfigurasi. Sistem membuat payment attempt dan memverifikasi hasil transaksi dari provider. Status `PAID`, invoice `LUNAS`, dan receipt hanya boleh ditetapkan setelah hasil transaksi terverifikasi.

SRD v1.0 tetap menjadi baseline. Requirement `SR-F-001` sampai `SR-F-084` dan `SR-NF-001` sampai `SR-NF-034` dipertahankan. Addendum ini memperjelas requirement yang terdampak dan menambahkan `SR-F-085` sampai `SR-F-100` serta `SR-NF-035` sampai `SR-NF-039`.

**Dokumen sumber:** [RabbitCare System Requirements Document v1.0](../requirements/RabbitCare-SRD.pdf)

## 1. Ruang lingkup dan prinsip refinement

Refinement mencakup:
- Booking request, admin approval/rejection, status transition, slot validation, authorization, audit, dan notification.
- Payment attempt melalui gateway, callback/webhook verification, payment status, invoice status, receipt, idempotency, dan monitoring.
- API requirements, data requirements, transaction rules, integration, logging, security, testing, traceability, dan system acceptance.
- Pembaruan diagram aktivitas, sequence, state, class, component, dan deployment dalam folder `plantuml/refinement/`.

Prinsip preservasi:
- Dokumen PDF SRD v1.0 tidak ditimpa.
- Diagram PlantUML asli di folder `plantuml/` tidak dihapus atau diubah oleh addendum ini.
- Requirement lama tetap dapat ditelusuri. Requirement baru menggunakan ID setelah nomor terakhir baseline.
- Hal yang belum diputuskan secara produk/operasional ditandai sebagai open decision, bukan ditetapkan diam-diam oleh desain teknis.

## 2. Temuan baseline dan perubahan utama

| Area SRD v1.0 | Baseline | Refinement v1.1 |
|---|---|---|
| Section 3–5: System Context / Logical Architecture | Booking Service, Billing Service, Payment Channel, Notification Provider. | Perjelas alur Admin Review pada Booking Service dan Payment Gateway Adapter/Callback Handler pada Billing & Payment. |
| SR-F-019–027 | Sistem menerima booking, mengunci slot, menyimpan status awal, mengirim event konfirmasi, cancel/reschedule. | Status awal menjadi `MENUNGGU_KONFIRMASI`; admin approval/rejection; slot divalidasi ulang saat approval. |
| API 7.3 Schedule and Booking | `POST /api/bookings`, `GET /api/bookings/{booking_id}`, `PATCH .../confirm`, cancel, reschedule. | Pertahankan endpoint confirm sebagai admin approval; tambahkan reject dan daftar booking pending. |
| SR-F-058–066 | Invoice, payment record, metode pembayaran, status invoice/booking, receipt, double-payment prevention. | Payment attempt gateway, provider reference, callback verification, idempotency, receipt setelah verified paid; booking lifecycle tidak diubah oleh payment. |
| API 7.8 Billing and Payment | `POST /api/payments`, GET payment, receipt. | Pertahankan API dasar; tambahkan gateway callback/webhook dan kontrak status payment. |
| Data Requirements | Invoice 1-to-many Payment. | Tegaskan satu record Payment mewakili satu payment attempt; tambahkan Payment Callback Event untuk deduplication/audit teknis. |
| Authorization Matrix | Admin CRUD Payment; Owner own read. | Owner memulai/melihat pembayaran miliknya; Admin monitoring dan exception handling, bukan mengubah status gateway menjadi paid secara manual. |
| Transaction Rules | Admin/payment channel memproses pembayaran, lalu update payment, invoice, booking, receipt. | Pisahkan transaksi database internal dari proses eksternal gateway; verified callback memicu payment/invoice update; tidak mengubah booking lifecycle. |
| Integration Requirements | Payment Channel mendukung cash, transfer, QR, card, other; gateway callback disebut secara umum. | Gateway menjadi jalur pembayaran MVP; provider-specific signature, reference, amount, currency, event deduplication, dan retry diturunkan. |
| SR-NF-016–018 | Atomic booking/payment, duplicate prevention, notification retry. | Tegaskan atomicity database, idempotency callback, duplicate event handling, dan secure secret handling. |

## 3. Logical architecture refinement

### 3.1 Booking components

**Booking Service**
- Menerima pengajuan booking dari Owner Portal.
- Memvalidasi token, role, dan kepemilikan pet.
- Meminta Schedule Service memeriksa slot.
- Membuat booking dengan status `MENUNGGU_KONFIRMASI`.
- Menyediakan review queue untuk admin.
- Memproses approval/rejection dengan pemeriksaan otorisasi dan audit.
- Memvalidasi ulang slot dalam transaksi saat admin menyetujui.

**Schedule Service**
- Menyediakan slot berdasarkan jadwal petugas dan kapasitas.
- Mengunci data jadwal/kapasitas selama transaksi pembuatan request atau approval.
- Memastikan tidak ada booking terkonfirmasi melebihi kapasitas slot.
- Mengembalikan `BOOKING_SLOT_UNAVAILABLE` bila kapasitas sudah tidak tersedia saat approval.

**Notification Service**
- Menerima event `booking_submitted`, `booking_approved`, dan `booking_rejected`.
- Mengirim notifikasi ke pemilik dan/atau admin sesuai kanal yang tersedia.
- Mencatat delivery status mengikuti SR-F-067 sampai SR-F-073 dan requirement notifikasi baseline.

### 3.2 Billing and Payment components

**Billing Service**
- Mengelola invoice dan item invoice.
- Menentukan nilai invoice yang masih harus dibayar.
- Menghubungkan invoice dengan satu atau lebih payment attempt.
- Menandai invoice `LUNAS` hanya setelah Payment Service menyatakan transaksi berhasil terverifikasi.

**Payment Service**
- Membuat payment attempt untuk invoice.
- Menghasilkan reference/idempotency key internal yang unik.
- Menyimpan provider transaction reference dan status.
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

### 4.1 Alur teknis pengajuan

1. Owner Portal mengirim `POST /api/bookings`.
2. API Gateway memvalidasi access token dan meneruskan request.
3. Booking Service memvalidasi kepemilikan pet, layanan, jadwal, dan data wajib.
4. Schedule Service memeriksa ketersediaan kapasitas.
5. Dalam transaksi database, sistem mengunci record jadwal/kapasitas selama proses insert, membuat booking ID dan booking code, serta menyimpan status `MENUNGGU_KONFIRMASI`.
6. Setelah commit, sistem menerbitkan event `booking_submitted` untuk notifikasi.
7. Pengajuan muncul di review queue admin.

### 4.2 Alur keputusan admin

**Approval**
- Hanya admin berwenang yang dapat menyetujui.
- Sistem memvalidasi ulang status booking masih `MENUNGGU_KONFIRMASI`.
- Sistem mengunci jadwal/kapasitas dan memeriksa ketersediaan kembali di dalam transaksi.
- Jika tersedia, sistem mengubah status menjadi `TERKONFIRMASI`, menyimpan `decided_by`, `decision_at`, dan audit log.
- Jika slot tidak lagi tersedia, sistem menolak aksi approval dengan `BOOKING_SLOT_UNAVAILABLE`; booking tidak boleh berubah menjadi `TERKONFIRMASI`.
- Setelah commit, sistem menerbitkan `booking_approved`.

**Rejection**
- Hanya admin berwenang yang dapat menolak.
- `rejection_reason` wajib diisi.
- Sistem mengubah status menjadi `DITOLAK`, menyimpan admin, waktu keputusan, alasan, dan audit log.
- Setelah commit, sistem menerbitkan `booking_rejected`.

### 4.3 Slot lock dan pending policy

SR-F-021 baseline mewajibkan penguncian slot selama proses pembuatan booking. Refinement mempertahankan lock tersebut sebagai lock transaksional untuk mencegah race condition pada proses request dan approval.

**Keputusan yang belum ditetapkan:** apakah booking berstatus `MENUNGGU_KONFIRMASI` menahan kapasitas slot setelah transaksi request selesai, berapa lama hold berlaku, dan bagaimana expiry/release dilakukan. Karena PRD menandai hal ini sebagai open decision, addendum ini tidak menetapkan durasi hold. Sistem tetap wajib memvalidasi ulang kapasitas saat admin melakukan approval dan mencegah booking terkonfirmasi melebihi kapasitas.

## 5. Payment gateway workflow

### 5.1 Initiation dan payment attempt

1. Owner membuka invoice yang belum lunas.
2. Owner memulai pembayaran melalui gateway.
3. Billing Service memvalidasi invoice, pemilik, outstanding amount, dan status `BELUM_DIBAYAR`.
4. Payment Service membuat payment attempt baru dengan status `PENDING`, internal reference, idempotency key, amount, currency, dan invoice ID.
5. Gateway Adapter meminta provider membuat transaksi/checkout.
6. Sistem menyimpan provider transaction reference dan mengembalikan instruksi/URL checkout yang disediakan provider.
7. Owner menyelesaikan pembayaran pada alur provider.

### 5.2 Callback verification

Callback/webhook tidak boleh dipercaya hanya karena request diterima atau browser diarahkan kembali ke RabbitCare.

Sebelum status diubah, sistem harus:
- Memvalidasi signature atau autentikasi callback sesuai spesifikasi provider.
- Memeriksa provider transaction reference dan internal payment reference.
- Memastikan invoice yang dirujuk sesuai dengan payment attempt.
- Memastikan amount dan currency sesuai dengan nilai yang diharapkan.
- Memeriksa status transaksi yang dikirim provider.
- Memeriksa event/provider reference agar callback yang sama tidak diproses dua kali.
- Mencatat hasil verifikasi dan processing status tanpa menyimpan secret atau data pembayaran sensitif.

Jika provider menyediakan server-to-server status inquiry, mekanisme tersebut dapat digunakan sebagai verifikasi tambahan sesuai dokumentasi provider. Pilihan final menunggu provider ditentukan.

### 5.3 Status update dan receipt

- Callback valid dengan hasil berhasil: payment menjadi `PAID`; invoice menjadi `LUNAS`; receipt diterbitkan setelah commit.
- Callback valid dengan hasil gagal: payment menjadi `FAILED`; invoice tetap `BELUM_DIBAYAR`.
- Callback valid dengan hasil dibatalkan: payment menjadi `CANCELLED`; invoice tetap `BELUM_DIBAYAR`.
- Callback pending: payment tetap `PENDING`; tidak ada receipt lunas.
- Callback tidak valid atau data mismatch: status payment tidak berubah menjadi `PAID`; event dicatat untuk monitoring/exception handling.
- Callback berulang untuk event yang sudah diproses: hasil diperlakukan idempotent dan tidak membuat payment/receipt ganda.

**Pemisahan lifecycle:** pembayaran hanya memperbarui Payment dan Invoice. Status Booking tidak berubah menjadi `LUNAS` dan tidak diperbarui oleh Payment Service. `BELUM_DIBAYAR`/`LUNAS` diperlakukan sebagai ringkasan status invoice, sedangkan booking mengikuti lifecycle operasionalnya sendiri.

### 5.4 Batas MVP dan open decisions

- Gateway adalah jalur pembayaran invoice dalam MVP sesuai keputusan produk.
- Provider dan metode pembayaran belum ditentukan.
- Pembayaran tunai/manual sebagai alternatif, partial payment, deposit, refund otomatis, dan multi-provider belum disetujui dalam scope ini.
- Status `REFUNDED` tetap dipertahankan sebagai status yang tercantum pada baseline, tetapi proses refund tidak otomatis menjadi fitur MVP.
- Retry payment attempt, settlement, reconciliation schedule, dan SLA callback harus disesuaikan setelah provider dipilih.

## 6. Functional requirements refinement

Requirement baseline `SR-F-001` sampai `SR-F-084` tetap berlaku. Requirement berikut ditambahkan:

| ID | System Requirement | Acceptance Criteria |
|---|---|---|
| SR-F-085 | Sistem harus menyimpan pengajuan booking pemilik dengan status awal `MENUNGGU_KONFIRMASI`. | Booking ID/code terbentuk; status awal tersimpan; pengajuan belum dianggap terkonfirmasi. |
| SR-F-086 | Sistem harus menyediakan daftar pengajuan booking yang menunggu keputusan admin. | Admin berwenang dapat memfilter dan membuka booking berstatus `MENUNGGU_KONFIRMASI`. |
| SR-F-087 | Sistem harus mengizinkan admin menyetujui atau menolak pengajuan booking. | Hanya admin berwenang; approval/rejection hanya berlaku pada status yang sah. |
| SR-F-088 | Sistem harus menyimpan identitas admin dan waktu keputusan booking. | `decided_by` dan `decision_at` tersimpan dan dapat diaudit. |
| SR-F-089 | Sistem harus mewajibkan alasan ketika admin menolak booking. | Rejection tanpa alasan ditolak dengan validation error. |
| SR-F-090 | Sistem harus menampilkan status keputusan booking kepada pemilik. | Owner hanya melihat booking miliknya dan status terbaru. |
| SR-F-091 | Sistem harus memvalidasi ulang kapasitas slot saat admin menyetujui booking. | Approval gagal bila kapasitas sudah tidak tersedia; tidak ada overbooking terkonfirmasi. |
| SR-F-092 | Sistem harus mencatat dan menerbitkan event pengajuan, approval, dan rejection. | Event hanya diterbitkan setelah perubahan database berhasil commit. |
| SR-F-093 | Sistem harus membuat payment attempt melalui payment gateway untuk invoice yang belum lunas. | Payment attempt terhubung ke invoice dan dimulai dengan status `PENDING`. |
| SR-F-094 | Sistem harus menyimpan internal payment reference dan provider transaction reference. | Setiap attempt dapat ditelusuri ke invoice dan transaksi provider. |
| SR-F-095 | Sistem harus memverifikasi callback gateway sebelum memperbarui status pembayaran. | Signature/authentication, reference, invoice, amount, currency, dan status diperiksa. |
| SR-F-096 | Sistem hanya boleh menetapkan payment `PAID` dan invoice `LUNAS` setelah transaksi terverifikasi berhasil. | Redirect browser atau callback tidak valid tidak dapat menandai invoice lunas. |
| SR-F-097 | Sistem harus menerbitkan receipt setelah payment `PAID` terverifikasi. | Receipt memiliki invoice ID dan payment reference; tidak dibuat untuk pending/failed/cancelled. |
| SR-F-098 | Sistem harus memproses callback gateway secara idempotent. | Callback/event yang sama tidak menggandakan payment, update invoice, atau receipt. |
| SR-F-099 | Sistem harus menyediakan monitoring transaksi gateway untuk admin berwenang. | Admin dapat melihat status, invoice, reference, waktu, dan transaksi yang perlu tindak lanjut. |
| SR-F-100 | Sistem harus memisahkan lifecycle booking dari lifecycle invoice/payment. | Update payment tidak mengubah status operasional booking. |

## 7. Nonfunctional requirements tambahan

Requirement `SR-NF-001` sampai `SR-NF-034` tetap berlaku. Tambahkan:

| ID | Requirement |
|---|---|
| SR-NF-035 | Callback gateway harus diverifikasi menggunakan mekanisme autentikasi/integritas yang didukung provider sebelum diproses. |
| SR-NF-036 | Pemrosesan callback dan update payment harus idempotent untuk mencegah transaksi ganda. |
| SR-NF-037 | Kredensial, secret, dan signing key gateway harus disimpan pada konfigurasi environment/secret store dan tidak ditanam pada source code. |
| SR-NF-038 | Log aplikasi tidak boleh menyimpan password, access token lengkap, signing secret, CVV, atau data pembayaran sensitif. |
| SR-NF-039 | Perubahan payment dan invoice di database harus konsisten secara transaksional; pemanggilan eksternal gateway dikelola sebagai proses terpisah dengan status dan retry/exception handling. |

Tidak ada target numerik performa, availability, atau SLA gateway baru yang ditetapkan oleh addendum ini.

## 8. API requirements refinement

API baseline dipertahankan kecuali penjelasan/penambahan berikut.

### 8.1 Schedule and Booking

| Method | Endpoint | Akses | Refinement |
|---|---|---|---|
| POST | `/api/bookings` | Owner | Membuat pengajuan dengan status `MENUNGGU_KONFIRMASI`. |
| GET | `/api/bookings/{booking_id}` | Owner pemilik booking; Admin/role sesuai authorization | Mengambil detail dan status booking. |
| GET | `/api/bookings?status=MENUNGGU_KONFIRMASI` | Admin | Daftar pengajuan untuk review queue. |
| PATCH | `/api/bookings/{booking_id}/confirm` | Admin | Endpoint baseline dipertahankan; bermakna approve setelah revalidation slot. |
| PATCH | `/api/bookings/{booking_id}/reject` | Admin | Endpoint baru; body memuat `rejection_reason`. |
| PATCH | `/api/bookings/{booking_id}/cancel` | Sesuai aturan baseline | Pembatalan mengikuti policy yang disepakati. |
| PATCH | `/api/bookings/{booking_id}/reschedule` | Sesuai aturan baseline | Reschedule mengikuti validasi jadwal dan status yang sah. |

### 8.2 Billing and Payment

| Method | Endpoint | Akses | Refinement |
|---|---|---|---|
| GET | `/api/invoices/by-booking/{booking_id}` | Owner terkait; Admin | Melihat invoice terkait booking. |
| POST | `/api/payments` | Owner terkait | Membuat payment attempt gateway untuk invoice yang belum lunas. |
| GET | `/api/payments/{payment_id}` | Owner terkait; Admin | Melihat status payment attempt. |
| GET | `/api/invoices/{invoice_id}/receipt` | Owner terkait; Admin | Mengambil receipt setelah paid terverifikasi. |
| POST | `/api/payments/gateway/callback` | Payment provider | Endpoint callback/webhook; autentikasi melalui signature/provider mechanism, bukan access token Owner. |

### 8.3 Contract dan response rules

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
  "provider_transaction_reference": "provider-reference",
  "amount": 150000,
  "currency": "IDR",
  "status": "PENDING"
}
```

Field dan format final provider callback harus mengikuti provider yang dipilih. Contoh di atas adalah ilustrasi kontrak internal, bukan payload provider tertentu.

Standard API response pada Section 8 SRD v1.0 tetap digunakan. Error response mengikuti format baseline.

### 8.4 Error codes tambahan

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
| `PAYMENT_EVENT_DUPLICATE` | Callback/event telah diproses; diperlakukan idempotent dan tidak membuat efek ganda. |

Error codes baseline seperti `SCHEDULE_NOT_AVAILABLE`, `BOOKING_INVALID_STATUS`, `INVOICE_ALREADY_PAID`, `PAYMENT_FAILED`, dan `VALIDATION_ERROR` tetap dipertahankan.

## 9. Data requirements dan validation

### 9.1 Booking data

Tambahan/penegasan atribut pada entitas Booking:
- `status`
- `requested_at`
- `decided_by_user_id` (nullable sampai keputusan dibuat)
- `decision_at` (nullable sampai keputusan dibuat)
- `rejection_reason` (nullable; wajib jika status `DITOLAK`)

Relasi Booking ke Owner, Pet, Schedule, CheckIn, Queue, MedicalRecord, dan Invoice tetap mengikuti baseline.

### 9.2 Payment data

Satu record `Payment` mewakili satu payment attempt. Satu Invoice dapat memiliki beberapa Payment attempt sampai satu attempt berhasil.

Atribut yang diperlukan:
- `payment_id`
- `invoice_id`
- `provider_code`
- `internal_reference`
- `provider_transaction_reference`
- `idempotency_key`
- `amount`
- `currency`
- `status`
- `initiated_at`
- `paid_at` (nullable)
- `failure_code` / `failure_message` (nullable)

### 9.3 Payment Callback Event

Tambahkan entitas teknis `PaymentCallbackEvent` untuk deduplication dan audit:
- `event_id` internal
- `provider_code`
- `provider_event_reference` (unik per provider jika tersedia)
- `payment_id`
- `event_type`
- `received_at`
- `verification_status`
- `processing_status`
- `payload_hash` atau metadata non-sensitif yang diperlukan untuk audit

Jangan menyimpan signing secret, CVV, password, access token lengkap, atau raw sensitive payment credentials.

### 9.4 Relasi dan constraint

- `Invoice 1 — 0..* Payment` (setiap payment merupakan attempt).
- `Payment 1 — 0..* PaymentCallbackEvent` (satu attempt dapat menerima beberapa event provider).
- `Booking 1 — 0..1 Invoice` tetap mengikuti baseline.
- Internal payment reference harus unik.
- Provider transaction reference harus unik dalam ruang lingkup provider bila provider menjamin keunikannya.
- Provider event reference harus unik dalam ruang lingkup provider bila tersedia.
- Amount harus sesuai outstanding invoice; currency harus sesuai konfigurasi invoice/provider.
- Hanya satu payment attempt yang boleh menyelesaikan invoice menjadi `LUNAS`.
- Repeated callback tidak boleh membuat receipt atau pembayaran sukses kedua.

## 10. Authorization matrix refinement

Hak akses lain tetap mengikuti Authorization Matrix pada Section 12 SRD v1.0. Area yang diperjelas:

| Fitur | Owner | Admin | Doctor | Groomer | Manager |
|---|---|---|---|---|---|
| Booking | Create / Read own | Read / Approve / Reject / manage sesuai policy | Read | Read | Read |
| Payment | Initiate / Read own / receipt own | Read / Monitor / exception handling | No | No | Read |
| Invoice | Read own | CRUD sesuai baseline | Read sesuai kebutuhan layanan | Read sesuai kebutuhan layanan | Read |

Admin tidak boleh mengubah status transaksi gateway menjadi `PAID` hanya melalui input manual. Penetapan `PAID` berasal dari hasil provider yang terverifikasi. Jika pembayaran manual/tunai akan dipertahankan sebagai alternatif, alur dan kontrolnya memerlukan keputusan produk serta requirement terpisah.

## 11. Transaction rules refinement

### 11.1 Booking request transaction

1. Validasi token dan role Owner.
2. Validasi kepemilikan pet.
3. Validasi layanan, jadwal, dan kapasitas.
4. Lock record slot/kapasitas selama transaksi.
5. Insert booking dengan status `MENUNGGU_KONFIRMASI`.
6. Commit.
7. Publish `booking_submitted` setelah commit.

Jika langkah validasi atau insert gagal, transaksi database dibatalkan. Notifikasi tidak dikirim sebagai konfirmasi booking terkonfirmasi.

### 11.2 Booking approval transaction

1. Validasi token dan role Admin.
2. Ambil booking dan pastikan status `MENUNGGU_KONFIRMASI`.
3. Lock jadwal/kapasitas dan validasi ulang slot.
4. Update status `TERKONFIRMASI`, admin, dan waktu keputusan.
5. Insert audit log.
6. Commit.
7. Publish `booking_approved`.

Jika slot tidak tersedia, transaksi approval dibatalkan dan booking tidak menjadi terkonfirmasi.

### 11.3 Booking rejection transaction

1. Validasi token dan role Admin.
2. Validasi status booking.
3. Validasi `rejection_reason`.
4. Update status `DITOLAK`, admin, waktu, dan alasan.
5. Insert audit log.
6. Commit.
7. Publish `booking_rejected`.

### 11.4 Payment transaction

**Internal initiation**
1. Validasi invoice dan kepemilikan.
2. Pastikan invoice belum `LUNAS`.
3. Buat payment attempt `PENDING` dengan reference/idempotency key.
4. Commit record internal.
5. Minta gateway membuat transaksi/checkout.
6. Simpan provider reference dan instruksi checkout.

**Verified callback processing**
1. Terima callback dan simpan event reference untuk deduplication.
2. Verifikasi signature/authentication.
3. Cocokkan payment reference, invoice, amount, currency, dan status.
4. Lock payment dan invoice terkait.
5. Validasi transisi status dan pastikan invoice belum diselesaikan oleh attempt lain.
6. Update Payment dan Invoice secara atomik.
7. Catat event processing/audit.
8. Commit.
9. Jika status `PAID`, generate receipt dan kirim notifikasi setelah commit.

Pemanggilan gateway eksternal tidak berada dalam satu transaksi ACID database yang sama. Konsistensi dijaga dengan payment state, idempotency, event processing, dan exception handling. Booking status tidak diperbarui pada transaksi payment.

## 12. Integration, logging, security, dan deployment

### 12.1 Payment gateway integration

- Gunakan adapter agar provider dapat diganti tanpa mengubah aturan domain Billing/Payment.
- Simpan konfigurasi endpoint, merchant identifier, dan secret pada environment/secret store.
- Gunakan HTTPS.
- Verifikasi callback sesuai dokumentasi provider.
- Catat reference, event ID, waktu diterima, hasil verifikasi, dan hasil pemrosesan.
- Jangan mengasumsikan satu provider atau metode tertentu sebelum keputusan provider dibuat.

### 12.2 Logging and monitoring

Pertahankan logging baseline (request ID, user ID, endpoint, response status, process time, internal error, notification delivery, payment callback, scheduler execution). Tambahkan:
- booking request ID, booking decision actor/time, dan hasil slot revalidation;
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
| Concurrency | Dua admin/request mencoba memakai kapasitas slot yang sama; tidak boleh ada overbooking terkonfirmasi. |
| Integration - Booking | Request → pending → admin approve/reject → owner status; notification event hanya setelah commit. |
| Integration - Payment | Create payment attempt, gateway success/pending/failed/cancelled, receipt after verified success. |
| Callback security | Invalid signature, invalid reference, amount mismatch, currency mismatch, callback replay, duplicate event. |
| Idempotency | Callback sama dikirim berulang; hanya satu payment outcome dan satu receipt. |
| Failure handling | Gateway timeout/unavailable, callback terlambat, status inquiry mismatch, retry/exception monitoring. |
| Regression | Check-in, queue, medical record, vaccination, grooming, reminder, reporting, audit log tetap berfungsi. |
| UAT | Owner, Admin, Doctor, Groomer, Manager sesuai peran dan acceptance criteria. |

## 14. Traceability matrix refinement

| Product Requirement | System Requirement |
|---|---|
| PR-13: Owner submits booking | SR-F-019–024, SR-F-085 |
| PR-15: Admin confirms/decides booking | SR-F-025, SR-F-087–092 |
| PR-56: Admin review queue | SR-F-086–087 |
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

Traceability baseline BR-01 sampai BR-52 dan PR-01 sampai PR-55 tetap mengikuti Section 20 SRD v1.0, dengan penjelasan status dan payment di addendum ini.

## 15. System acceptance criteria tambahan

Sistem memenuhi refinement ini apabila:
1. Owner dapat mengirim booking dan status awalnya `MENUNGGU_KONFIRMASI`.
2. Hanya Admin berwenang yang dapat approve/reject booking.
3. Rejection mewajibkan alasan; identitas admin dan waktu keputusan tercatat.
4. Approval memvalidasi ulang kapasitas slot dan tidak menghasilkan overbooking.
5. Owner dapat melihat status booking miliknya.
6. Payment attempt dibuat melalui gateway dan terhubung ke invoice yang benar.
7. Callback diverifikasi terhadap signature/authentication, reference, invoice, amount, currency, dan status.
8. Hanya transaksi sukses terverifikasi yang mengubah Payment menjadi `PAID` dan Invoice menjadi `LUNAS`.
9. Receipt hanya diterbitkan setelah status `PAID` terverifikasi.
10. Callback duplikat/replay tidak menggandakan payment atau receipt.
11. Admin dapat memonitor transaksi dan exception tanpa mengubah hasil provider secara manual.
12. Payment tidak mengubah booking lifecycle.
13. Seluruh requirement baseline yang tidak berubah tetap lulus regression test dan UAT.

## 16. Open decisions sebelum technical sign-off

- Provider gateway, merchant account, metode pembayaran, currency, biaya, settlement, dan dokumentasi callback.
- Apakah server-to-server status inquiry tersedia/diwajibkan oleh provider.
- Kebijakan booking pending slot hold/expiry dan SLA approval admin.
- Apakah pembayaran tunai/manual tersedia sebagai alternatif gateway.
- Retry payment attempt, rekonsiliasi, refund, deposit, partial payment, dan multi-provider.
- Aturan invoice expiry dan perubahan invoice setelah payment attempt dibuat.
- Notification channel/template untuk booking submitted/approved/rejected dan payment outcome.
- Target operasional callback processing dan gateway availability setelah provider dipilih.

## 17. Diagram refinement

Diagram asli tetap berada di `plantuml/`. Versi refinement v1.1 berada di `plantuml/refinement/` agar baseline dapat ditelusuri.

- [Activity — Booking Service v1.1](../../plantuml/refinement/05_activity_booking_service_v1.1.puml)
- [Activity — Payment v1.1](../../plantuml/refinement/09_activity_payment_v1.1.puml)
- [Sequence — Booking v1.1](../../plantuml/refinement/11_sequence_booking_v1.1.puml)
- [Sequence — Payment Gateway v1.1](../../plantuml/refinement/15_sequence_payment_v1.1.puml)
- [Class Diagram v1.1](../../plantuml/refinement/17_class_diagram_v1.1.puml)
- [Booking State v1.1](../../plantuml/refinement/18_state_booking_v1.1.puml)
- [Component Diagram v1.1](../../plantuml/refinement/19_component_diagram_v1.1.puml)
- [Deployment Diagram v1.1](../../plantuml/refinement/20_deployment_diagram_v1.1.puml)
- [Payment State v1.1](../../plantuml/refinement/21_state_payment_v1.1.puml)

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation. Addendum ini tidak menetapkan provider gateway, slot hold duration, SLA, atau metode pembayaran yang belum disetujui.

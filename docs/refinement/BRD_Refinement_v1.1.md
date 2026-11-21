# RabbitCare - BRD Refinement Addendum v1.1

**Tanggal refinement:** 20 November 2026  
**Dokumen sumber:** Business Requirements Document (BRD) RabbitCare v1.0  
**Status:** Proposed refinement / Draft - pending stakeholder validation  
**Jenis perubahan:** Addendum terhadap baseline, bukan penggantian dokumen asli

## Ringkasan keputusan

Dua keputusan ruang lingkup yang menjadi dasar refinement ini:

1. **Owner-submitted booking requires admin approval.** Booking yang diajukan pemilik tidak langsung berstatus terkonfirmasi; admin meninjau dan menyetujui atau menolaknya.
2. **Payment gateway termasuk dalam MVP.** Integrasi payment gateway menjadi bagian dari ruang lingkup awal RabbitCare.

Dokumen BRD v1.0 tetap dipertahankan tanpa perubahan. Addendum ini hanya memperbarui ketentuan yang terdampak. Ketentuan lain yang tidak disebutkan tetap mengikuti BRD v1.0.

> **Status validasi:** keputusan arah produk di atas telah dipilih untuk refinement project. Ketentuan operasional rinci yang masih memerlukan persetujuan stakeholder ditandai sebagai *open decision* atau *proposed control*.

## 1. Tujuan dan cara membaca addendum

Addendum ini menyelaraskan kebutuhan bisnis RabbitCare dengan keputusan terbaru mengenai proses booking dan pembayaran. Dokumen ini tidak mengubah seluruh BRD dan tidak menyatakan bahwa kebutuhan telah disetujui oleh pihak klinik.

Gunakan BRD v1.0 sebagai baseline. Jika terdapat perbedaan pada bagian yang secara eksplisit dibahas di addendum ini, gunakan rumusan refinement v1.1 sebagai usulan terbaru untuk validasi. Untuk bagian lain, tetap gunakan BRD v1.0.

## 2. Ringkasan perubahan terhadap BRD v1.0

| Bagian BRD v1.0 | Baseline | Refinement v1.1 |
|---|---|---|
| Informasi dokumen | Versi 1.0, Draft | BRD v1.0 tetap menjadi baseline; addendum ini dicatat sebagai refinement v1.1. |
| 6.2 Admin / Resepsionis | Admin melakukan konfirmasi booking dan konfirmasi pembayaran. | Admin meninjau, menyetujui, atau menolak booking. Untuk transaksi gateway, admin memantau status dan menangani pengecualian; status sukses berasal dari hasil gateway yang tervalidasi. |
| 6.6 Penyedia eksternal | Payment channel atau payment gateway disebut sebagai kemungkinan penyedia eksternal. | Payment gateway menjadi dependensi integrasi dalam MVP; provider spesifik belum dipilih. |
| 8. Kondisi target | Pemilik melakukan booking; sistem menyediakan jadwal dan mencegah benturan slot. | Booking yang diajukan pemilik melewati keputusan admin sebelum terkonfirmasi. |
| 9.1 Ruang lingkup | Tagihan dan pembayaran termasuk dalam ruang lingkup. | Integrasi payment gateway secara eksplisit termasuk dalam MVP. |
| 10.2 Jadwal dan booking | BR-08 mengizinkan pemilik atau admin membuat booking. | BR-08 tetap berlaku; ditambahkan kebutuhan persetujuan admin untuk booking yang diajukan pemilik. |
| 10.7 Tagihan dan pembayaran | Tagihan, status pembayaran, nomor transaksi, dan bukti pembayaran. | Kebutuhan tersebut tetap berlaku; ditambahkan integrasi gateway dan kontrol agar notifikasi transaksi tidak diproses ganda. |
| 11. Business Rules | BRule-10 menyatakan status lunas setelah pembayaran dikonfirmasi. | BRule-10 diperjelas untuk transaksi gateway; ditambahkan aturan persetujuan booking dan pemrosesan notifikasi transaksi. |
| 12. Business Process Target | Booking diikuti konfirmasi, check-in, layanan, tagihan, dan pembayaran. | Disisipkan tahap review admin pada booking dan pembayaran melalui gateway setelah invoice diterbitkan. |
| 13. Asumsi | Pembayaran manual dapat dicatat tanpa payment gateway. | Asumsi ini perlu direvisi: gateway masuk MVP; ketersediaan pembayaran tunai/manual sebagai alternatif masih harus diputuskan. |
| 14-16. Risiko, keberhasilan, penerimaan | Risiko, indikator, dan kriteria penerimaan umum. | Ditambahkan kontrol dan kriteria terkait approval booking dan transaksi gateway. |

## 3. Penyesuaian peran stakeholder

### 3.1 Pemilik kelinci

Pemilik tetap dapat:
- Membuat akun dan profil kelinci.
- Memilih layanan, petugas, tanggal, dan slot yang tersedia.
- Mengajukan booking dan melihat status keputusan admin.
- Menerima informasi persetujuan atau penolakan booking.
- Melihat tagihan dan memulai pembayaran melalui payment gateway.
- Melihat status pembayaran dan bukti transaksi setelah pembayaran berhasil.

### 3.2 Admin atau resepsionis

Tanggung jawab admin diperbarui menjadi:
- Mengelola data administratif pemilik dan pasien.
- Mengelola jadwal dokter dan groomer.
- Meninjau permintaan booking.
- Menyetujui atau menolak booking yang diajukan pemilik.
- Menyimpan alasan penolakan serta identitas dan waktu keputusan.
- Melakukan check-in dan mengelola antrean.
- Memeriksa rincian dan menerbitkan invoice sesuai proses klinik.
- Memantau status transaksi payment gateway dan menindaklanjuti pengecualian operasional.

Untuk transaksi melalui gateway, admin tidak menetapkan status sukses hanya berdasarkan bukti yang dikirim pemilik. Status transaksi harus berasal dari hasil provider yang telah diverifikasi sistem. Jika klinik tetap ingin menerima pembayaran tunai/manual, prosedur dan kewenangan pencatatannya perlu diputuskan tersendiri.

### 3.3 Penyedia layanan eksternal

Payment gateway menjadi penyedia eksternal yang dibutuhkan untuk alur pembayaran MVP. Provider, metode pembayaran, merchant account, biaya, dan ketentuan layanannya belum ditentukan.

## 4. Business requirements tambahan

BR-01 sampai BR-52 pada BRD v1.0 tetap berlaku kecuali bagian yang secara eksplisit diperjelas dalam addendum ini.

| ID | Business Requirement tambahan | Prioritas |
|---|---|---|
| BR-53 | Sistem harus menempatkan booking yang diajukan pemilik pada status menunggu konfirmasi sampai admin menyetujui atau menolaknya. | Must |
| BR-54 | Sistem harus mengintegrasikan payment gateway dalam MVP dan menyediakan alur pembayaran digital untuk invoice yang diterbitkan. | Must |
| BR-55 | Sistem harus menyimpan alasan penolakan booking, identitas admin yang mengambil keputusan, dan waktu keputusan. | Must |
| BR-56 | Sistem harus mencegah notifikasi/callback pembayaran yang sama menghasilkan pemrosesan transaksi atau penerbitan bukti pembayaran lebih dari satu kali. | Must |

**Catatan:** BR-56 menyatakan hasil bisnis yang harus dijaga. Mekanisme teknis validasi callback dan idempotency akan dijabarkan lebih rinci pada SRD.

## 5. Business rules yang diperbarui dan ditambahkan

| ID | Rumusan | Status |
|---|---|---|
| BRule-10 | Status lunas untuk pembayaran melalui gateway hanya diberikan setelah provider menyatakan transaksi berhasil dan hasilnya tervalidasi oleh sistem. | Revisi |
| BRule-15 | Booking yang diajukan pemilik harus disetujui atau ditolak oleh admin sebelum berstatus terkonfirmasi. | Baru |
| BRule-16 | Notifikasi/callback pembayaran dari transaksi yang sama tidak boleh menghasilkan settlement atau bukti pembayaran ganda. | Baru |

BRule-01 sampai BRule-09 dan BRule-11 sampai BRule-14 tetap mengikuti BRD v1.0, dengan penyesuaian BRule-10 seperti di atas.

### Proposed control - penahanan slot booking

BRD v1.0 BRule-03 menyatakan bahwa satu slot hanya dapat digunakan oleh satu booking aktif. Untuk mencegah benturan selama admin meninjau booking, usulan kontrolnya adalah booking berstatus menunggu konfirmasi tetap menahan slot. Jika booking ditolak atau dibatalkan, slot dilepaskan.

**Hal ini masih perlu validasi stakeholder**, khususnya batas waktu penahanan slot dan kebijakan otomatis jika admin belum mengambil keputusan. Addendum ini tidak menetapkan SLA atau expiry time.

## 6. Perubahan kondisi target dan alur bisnis

### 6.1 Alur booking target

1. Pemilik memilih profil kelinci, layanan, petugas, tanggal, dan slot.
2. Sistem memeriksa ketersediaan dan mencatat permintaan booking.
3. Booking yang diajukan pemilik berstatus `MENUNGGU_KONFIRMASI`.
4. Admin meninjau detail permintaan dan jadwal.
5. Jika disetujui, booking berubah menjadi `TERKONFIRMASI` dan pemilik menerima notifikasi.
6. Jika ditolak, booking berubah menjadi `DITOLAK`; alasan penolakan, admin, dan waktu keputusan dicatat; pemilik menerima notifikasi.
7. Booking yang telah terkonfirmasi dapat dilanjutkan ke check-in, antrean, dan pelayanan.

Booking yang dibuat admin atas nama pemilik memerlukan aturan tersendiri: apakah tetap harus menunggu persetujuan atau dapat langsung dikonfirmasi.

### 6.2 Alur pembayaran target

1. Layanan selesai dan hasil layanan dicatat.
2. Sesuai BRule-09 pada BRD v1.0, tagihan dibuat setelah layanan tercatat.
3. Admin memeriksa rincian dan menerbitkan invoice.
4. Pemilik membuka invoice dan memulai pembayaran melalui payment gateway.
5. Sistem mencatat referensi percobaan pembayaran dan menampilkan instruksi/halaman pembayaran dari provider.
6. Provider memproses transaksi dan mengirimkan hasil transaksi kepada sistem.
7. Sistem memverifikasi hasil transaksi sebelum memperbarui status.
8. Jika transaksi berhasil dan valid, pembayaran dan invoice dicatat lunas serta bukti pembayaran diterbitkan.
9. 9. Jika transaksi gagal, invoice tetap belum lunas. Kebijakan percobaan ulang dan masa berlaku pembayaran mengikuti ketentuan yang akan ditetapkan bersama stakeholder dan payment gateway provider.

Detail teknis checkout session, callback, signature, idempotency, dan rekonsiliasi dijabarkan pada SRD, bukan ditetapkan sebagai detail implementasi di BRD.

## 7. Penyesuaian ruang lingkup MVP

### Termasuk dalam MVP
- Seluruh ruang lingkup bisnis BRD v1.0 yang tidak diubah oleh addendum ini.
- Review, persetujuan, dan penolakan booking oleh admin.
- Pencatatan alasan penolakan dan jejak keputusan admin.
- Integrasi payment gateway untuk pembayaran invoice.
- Pemantauan status transaksi gateway dan penerbitan bukti pembayaran berdasarkan transaksi berhasil yang tervalidasi.

### Belum ditetapkan sebagai bagian MVP
- Provider payment gateway tertentu.
- Refund otomatis dan pembayaran parsial.
- Pembayaran deposit atau pembayaran sebelum layanan.
- Pembayaran tunai/manual sebagai alternatif di luar gateway.
- Prosedur rekonsiliasi khusus jika notifikasi provider terlambat atau tidak diterima.

Item pada daftar kedua merupakan keputusan yang masih terbuka, bukan pernyataan bahwa semuanya pasti dilarang atau dikecualikan.

## 8. Asumsi dan dependensi bisnis

- Klinik memiliki koneksi internet untuk menggunakan sistem dan mengakses layanan provider.
- Klinik akan memilih provider payment gateway dan menyediakan merchant account serta kredensial yang diperlukan sebelum integrasi.
- Provider yang dipilih harus mendukung metode pembayaran dan mekanisme pemberitahuan status transaksi yang dapat diverifikasi.
- Jadwal dokter dan groomer dikelola oleh pihak klinik.
- Pemilik memiliki nomor telepon atau email aktif untuk menerima notifikasi.
- Ketersediaan pembayaran tunai/manual di samping gateway masih memerlukan keputusan stakeholder.
- Batas waktu persetujuan booking dan expiry slot belum ditetapkan.

## 9. Risiko bisnis tambahan

| Risiko | Dampak | Respons yang perlu disiapkan |
|---|---|---|
| Admin terlambat meninjau booking | Pemilik menunggu kepastian dan kapasitas slot sulit dikelola. | Tetapkan SLA review dan mekanisme eskalasi bersama stakeholder. |
| Provider gateway mengalami gangguan | Pemilik tidak dapat menyelesaikan pembayaran sementara. | Tampilkan status yang jelas dan tentukan prosedur retry atau penanganan alternatif. |
| Notifikasi status transaksi terlambat/tidak diterima | Status pembayaran di sistem belum mencerminkan hasil terbaru. | Siapkan monitoring dan prosedur rekonsiliasi dengan provider. |
| Notifikasi transaksi duplikat | Risiko pencatatan atau penerbitan bukti ganda. | Pastikan transaksi yang sama hanya diproses satu kali. |
| Status transaksi tidak cocok dengan invoice | Risiko kesalahan pencatatan pembayaran. | Verifikasi referensi transaksi dan nilai pembayaran sebelum status lunas ditetapkan. |

## 10. Indikator keberhasilan dan kriteria penerimaan tambahan

Indikator keberhasilan yang sudah tercantum pada BRD v1.0 tetap berlaku. Addendum ini menambahkan indikator kualitatif berikut tanpa mengarang target persentase baru:

- Setiap booking yang diajukan pemilik memiliki status keputusan yang dapat ditelusuri.
- Persetujuan atau penolakan booking dapat ditelusuri ke admin dan waktu keputusan.
- Transaksi gateway yang berhasil memiliki referensi transaksi dan bukti pembayaran.
- Status lunas tidak diberikan sebelum hasil transaksi gateway dinyatakan berhasil dan tervalidasi.
- Notifikasi transaksi yang sama tidak menghasilkan pencatatan atau bukti pembayaran ganda.

### Kriteria penerimaan booking
- Pemilik dapat mengajukan booking.
- Booking pemilik tidak langsung berstatus `TERKONFIRMASI`.
- Admin dapat menyetujui atau menolak booking.
- Penolakan menyimpan alasan dan jejak keputusan.
- Pemilik dapat mengetahui hasil keputusan booking.

### Kriteria penerimaan pembayaran
- Pemilik dapat memulai pembayaran invoice melalui payment gateway.
- Status transaksi dapat ditelusuri ke referensi provider.
- Invoice hanya dinyatakan lunas setelah hasil sukses tervalidasi.
- Bukti pembayaran diterbitkan setelah transaksi berhasil.
- Notifikasi transaksi duplikat tidak membuat pencatatan atau bukti pembayaran ganda.

## 11. Keputusan yang masih terbuka

Sebelum BRD dianggap disetujui stakeholder, hal berikut perlu diputuskan:

1. Provider payment gateway, merchant account, metode pembayaran, dan biaya transaksi.
2. Apakah pembayaran tunai/manual tetap tersedia sebagai alternatif di klinik.
3. SLA admin untuk meninjau booking.
4. Batas waktu penahanan slot dan kebijakan expiry booking yang belum diproses.
5. Perlakuan booking yang dibuat admin atas nama pemilik.
6. Kebijakan refund, pembatalan transaksi, dan rekonsiliasi jika notifikasi provider terlambat/tidak diterima.
7. Apakah pembayaran hanya dilakukan setelah layanan atau deposit diperlukan pada fase berikutnya.
8. Kebijakan payment retry, termasuk batas percobaan, masa berlaku invoice, dan penanganan transaksi pending atau expired.

## 12. Referensi ke BRD baseline

Addendum ini diturunkan dari bagian-bagian berikut pada BRD RabbitCare v1.0:

- Bagian 1 - Informasi Dokumen.
- Bagian 6.2 dan 6.6 - Stakeholder admin dan penyedia layanan eksternal.
- Bagian 8 dan 9 - Kondisi target dan ruang lingkup bisnis.
- Bagian 10.2 dan 10.7 - Business requirements jadwal/booking dan tagihan/pembayaran.
- Bagian 11 - Business Rules, terutama BRule-03, BRule-09, dan BRule-10.
- Bagian 12 - Business Process Target.
- Bagian 13 - Asumsi.
- Bagian 14 sampai 16 - Risiko, indikator keberhasilan, dan kriteria penerimaan.

## 13. Status dokumen

BRD v1.0 tetap menjadi dokumen sumber dan tidak diubah. Addendum v1.1 ini merupakan usulan refinement untuk menjaga konsistensi dengan keputusan booking admin approval dan payment gateway pada MVP.

**Status akhir:** Proposed refinement / Draft - pending stakeholder validation. Dokumen ini belum merupakan persetujuan resmi dari pemilik atau manajemen klinik.

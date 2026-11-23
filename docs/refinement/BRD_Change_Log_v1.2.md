# RabbitCare — BRD Change Log v1.2

**Tanggal:** 23 November 2026  
**Baseline:** BRD v1.0 (Draft) dan BRD Refinement v1.1  
**Status:** Proposed refinement / Draft — pending stakeholder validation

## Ringkasan perubahan

| Area | Perubahan v1.2 |
|---|---|
| Booking approval | Validasi ulang kapasitas sebelum approval ditegaskan sebagai kebutuhan bisnis. |
| Pending slot | Persistent slot hold, durasi, dan expiry tetap menjadi open decision; proposed hold pada v1.1 tidak diperlakukan sebagai aturan final. |
| Visit | Status kunjungan pasien dicatat terpisah dari status Booking dan Service Execution. |
| Service Execution | Setiap pelaksanaan layanan memiliki status tersendiri. |
| Lifecycle | Booking, Visit, Service Execution, Invoice, dan Payment diperlakukan sebagai lifecycle berbeda. |
| Booking traceability | Identitas, peran, dan kanal/sumber pembuat booking harus dapat ditelusuri. |

## Requirement dan business rule baru

| ID | Perubahan |
|---|---|
| BR-57 | Validasi ulang kapasitas jadwal, petugas, dan layanan sebelum approval. |
| BR-58 | Status Visit dicatat terpisah dari Booking dan Service Execution. |
| BR-59 | Status setiap Service Execution dicatat terpisah dari Visit dan Service Execution lainnya. |
| BR-60 | Identitas, peran, dan kanal/sumber pembuat booking dicatat. |
| BRule-17 | Perubahan pada satu lifecycle tidak otomatis mengubah lifecycle lainnya kecuali ditetapkan oleh aturan bisnis. |

## Klarifikasi dan open decisions

- Sistem wajib memvalidasi ulang kapasitas sebelum approval.
- Apakah pengajuan pending menahan slot secara persisten masih perlu validasi.
- Durasi hold, expiry, dan SLA approval belum ditetapkan.
- Waktu pembentukan Visit, jumlah Service Execution per Visit, serta status dan transisinya belum ditetapkan.
- Kebijakan approval untuk booking yang dibuat admin atas nama pemilik tetap terbuka.

## Traceability

- BR-57 → PR-14, PR-15, PR-59; SR-F-091.
- BR-58 → SR-F-101.
- BR-59 → SR-F-102.
- BR-60 → SR-F-103.
- BRule-17 → PR-45; SR-F-100–102.

Product-level alignment untuk BR-58 sampai BR-60 perlu diselesaikan pada refinement PRD berikutnya.

## Preservasi baseline

PDF BRD v1.0 dan addendum BRD v1.1 tetap dipertahankan. Addendum v1.2 merupakan dokumen tambahan dan tidak menghapus atau mengganti dokumen sebelumnya.

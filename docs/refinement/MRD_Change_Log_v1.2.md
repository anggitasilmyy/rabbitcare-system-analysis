# RabbitCare — MRD Change Log v1.2

**Tanggal:** 23 November 2026  
**Baseline:** MRD v1.0 (Draft) dan MRD Refinement v1.1  
**Status:** Proposed refinement / Draft — pending stakeholder validation

## Ringkasan perubahan

| Area | Perubahan v1.2 |
|---|---|
| MR-21 | Diperjelas dengan pemeriksaan jadwal/kapasitas dan validasi ulang sebelum approval. |
| Pending slot | Persistent slot hold, durasi, dan expiry tetap menjadi open decision. |
| MR-24 | Klinik dapat memantau Visit dan Service Execution secara terpisah. |
| MR-25 | Klinik dapat menelusuri identitas, peran, dan kanal/sumber pembuat booking. |
| User journey | Alur booking dan kunjungan diperjelas agar status Visit tidak tercampur dengan status pelaksanaan layanan. |
| Success metrics | Indikator kapasitas approval, Visit, Service Execution, dan booking source ditambahkan tanpa target numerik baru. |

## Penambahan kebutuhan pasar

| ID | Kebutuhan |
|---|---|
| MR-24 | Pemantauan status Visit secara terpisah dari status setiap Service Execution. |
| MR-25 | Traceability identitas, peran, dan kanal/sumber pembuat booking. |

Prioritas MR-24 dan MR-25 diusulkan sebagai Must Have untuk MVP dan tetap memerlukan validasi stakeholder.

## Open decisions

- Apakah booking pending menahan slot secara persisten, durasi hold, expiry, dan SLA admin.
- Penanganan jika kapasitas tidak tersedia pada saat approval.
- Kebijakan booking yang dibuat admin atas nama pemilik.
- Waktu pembentukan Visit dan jumlah Service Execution dalam satu Visit.
- Nama status/transisi Visit dan Service Execution.
- Pembagian kewenangan untuk memperbarui status dan kebutuhan informasi yang ditampilkan kepada pemilik.

## Traceability

- MR-21 → BR-53/BR-57; PR-14/PR-15/PR-59; SR-F-085–092.
- MR-24 → BR-58/BR-59; SR-F-101–102.
- MR-25 → BR-60; SR-F-103.

Product-level alignment untuk MR-24 dan MR-25 perlu diselesaikan pada refinement PRD berikutnya.

## Preservasi baseline

PDF MRD v1.0 dan addendum MRD v1.1 tetap dipertahankan. Addendum v1.2 merupakan dokumen tambahan dan tidak menghapus atau mengganti dokumen sebelumnya.

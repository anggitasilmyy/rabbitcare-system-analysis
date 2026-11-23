# RabbitCare — Gap Analysis Change Log v1.2

**Tanggal:** 23 November 2026  
**Baseline:** RabbitCare Gap Analysis v1.0 dan Gap Analysis Refinement v1.1  
**Dokumen terkait:** BRD/MRD Refinement v1.1, PRD Refinement v1.2, SRD Refinement v1.2 — Revised, dan PlantUML Refinement v1.2  
**Status:** Draft — pending stakeholder validation

## Purpose

Mencatat klarifikasi terarah pada Gap Analysis v1.2. Klarifikasi ini menggunakan kebutuhan bisnis dan pasar yang telah tersedia pada baseline serta addendum v1.1, dan menyelaraskan pemodelan lifecycle dengan PRD/SRD v1.2. Baseline v1.0 dan addendum v1.1 tetap dipertahankan.

## Change summary

| Area | Kondisi sebelumnya | Perubahan v1.2 |
|---|---|---|
| Lifecycle classification | Visit dan Service Execution masih dapat dibaca sebagai satu kelompok `Visit/service`. | Visit dan setiap Service Execution dijelaskan sebagai lifecycle yang berbeda. |
| Cross-process status | Booking, Visit, Service Execution, Invoice, dan Payment berpotensi dipahami sebagai satu status gabungan. | Kelima lifecycle dipisahkan dan dihubungkan melalui referensi data. |
| Booking provenance | Sumber dan aktor pembuat booking belum dijabarkan sebagai gap tersendiri. | Identitas aktor, snapshot peran, dan kanal/sumber pembuat booking ditambahkan sebagai kebutuhan traceability dan dipetakan ke SR-F-103. |
| Medical record | Hubungan rekam medis perlu tetap konsisten setelah Visit diperkenalkan. | Pet ID dipertahankan sebagai penghubung utama rekam medis; Visit dapat menjadi konteks encounter tambahan. |
| Billing traceability | Hubungan layanan yang dilakukan dengan item invoice memerlukan kejelasan. | Hubungan Service Execution dan InvoiceItem dicatat sebagai hal yang perlu ditetapkan pada detailed billing design. |
| Requirements traceability | Hubungan antara kebutuhan proses baseline dan pemisahan lifecycle belum dijelaskan secara eksplisit. | Visit dan Service Execution dipetakan ke requirement yang tersedia serta PR-66/PR-67 dan SR-F-101/SR-F-102. Booking provenance dipetakan ke requirement booking/audit yang tersedia dan SR-F-103. |

## Traceability clarification

Requirement yang digunakan dalam addendum ini adalah ID yang memang tersedia pada repository:

- **Visit:** BR-12–BR-16, BR-43, BR-46; MR-07, MR-08, MR-18; PR-66; SR-F-101.
- **Service Execution:** BR-17–BR-32; MR-09–MR-12, MR-17; PR-67; SR-F-102.
- **Medical history:** BR-03, BR-17; MR-09, MR-17; SR-F-009.
- **Billing:** BR-33–BR-37; MR-14; SR-F-058–SR-F-059.
- **Booking provenance:** BR-08, BR-51; MR-04, MR-19; PR-13, PR-15; SR-F-103.

BR/MR/PR references above are existing baseline or v1.1 requirements. PR-66 and PR-67 are added in PRD v1.2; SR-F-101 to SR-F-103 are added in SRD v1.2. Booking creator metadata is specified at system level by SR-F-103; no separate product requirement for that metadata or BRD/MRD v1.2 addendum is included in this repository snapshot.

## Scope preserved

Analisis As-Is, 18 area proses, SWOT, Fishbone, root-cause priority, implementation roadmap, gap closure plan, dan indikator keberhasilan dari baseline serta addendum v1.1 tetap berlaku kecuali pada klarifikasi eksplisit di v1.2.

## Versioning note

- Gap Analysis v1.0 tetap menjadi baseline.
- Gap Analysis Refinement v1.1 tidak ditimpa.
- Gap Analysis Refinement v1.2 merupakan addendum terarah, bukan pengganti keseluruhan dokumen analisis.
- Status tetap **Draft — pending stakeholder validation**; tidak ada klaim persetujuan stakeholder atau implementasi.

**Status akhir:** Draft — pending stakeholder validation.

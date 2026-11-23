# RabbitCare — Gap Analysis Change Log v1.2

**Tanggal:** 23 November 2026  
**Baseline:** RabbitCare Gap Analysis v1.0 dan Gap Analysis Refinement v1.1  
**Dokumen penyelaras:** BRD/MRD/PRD Refinement v1.2 dan SRD Refinement v1.2 — Revised  
**Status:** Draft — pending stakeholder validation

## Purpose

Mencatat klarifikasi Gap Analysis setelah penyelarasan kebutuhan bisnis, pasar, produk, dan sistem pada v1.2. Baseline v1.0 dan addendum v1.1 tetap dipertahankan.

## Change summary

| Area | Kondisi sebelumnya | Perubahan v1.2 |
|---|---|---|
| Lifecycle classification | Visit dan Service Execution masih dapat dibaca sebagai satu kelompok `Visit/service`. | Visit dan setiap Service Execution dijelaskan sebagai lifecycle yang berbeda. |
| Cross-process status | Booking, Visit, Service Execution, Invoice, dan Payment berpotensi dipahami sebagai satu status gabungan. | Kelima lifecycle dipisahkan dan dihubungkan melalui referensi data. |
| Booking provenance | Sumber dan aktor pembuat booking belum dijabarkan sebagai gap tersendiri. | Identitas aktor, snapshot peran, dan kanal/sumber pembuat booking ditambahkan sebagai kebutuhan traceability. |
| Medical record | Hubungan rekam medis perlu tetap konsisten setelah Visit diperkenalkan. | Pet ID dipertahankan sebagai penghubung utama rekam medis; Visit dapat menjadi konteks encounter tambahan. |
| Billing traceability | Hubungan layanan yang dilakukan dengan item invoice memerlukan kejelasan. | Hubungan Service Execution dan InvoiceItem dicatat sebagai hal yang perlu ditetapkan pada desain/billing. |
| Requirements traceability | Gap Analysis v1.1 belum memetakan lifecycle Visit/Service Execution dan booking provenance ke requirement v1.2. | BR-58–BR-60, MR-24–MR-25, PR-66–PR-68, dan SR-F-101–SR-F-103 ditautkan. |

## Scope preserved

Analisis As-Is, 18 area proses, SWOT, Fishbone, root-cause priority, implementation roadmap, gap closure plan, dan indikator keberhasilan dari baseline serta addendum v1.1 tetap berlaku kecuali pada klarifikasi eksplisit di v1.2.

## Versioning note

- Gap Analysis v1.0 tetap menjadi baseline.
- Gap Analysis Refinement v1.1 tidak ditimpa.
- Gap Analysis Refinement v1.2 merupakan addendum terarah, bukan pengganti keseluruhan dokumen analisis.
- Status tetap **Draft — pending stakeholder validation**; tidak ada klaim persetujuan stakeholder atau implementasi.

**Status akhir:** Draft — pending stakeholder validation.

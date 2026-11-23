# RabbitCare — PRD Change Log v1.2

**Tanggal:** 23 November 2026  
**Baseline:** PRD v1.0 dan PRD Refinement v1.1  
**Status:** Proposed refinement / Draft — pending stakeholder validation

## Tujuan

Mencatat klarifikasi product requirements setelah penambahan BRD/MRD Refinement v1.2 dan penyempurnaan SRD Refinement v1.2, tanpa mengubah dokumen baseline atau versi sebelumnya.

## Ringkasan perubahan

| Area | Perubahan v1.2 |
|---|---|
| Product States | Status Booking dipisahkan dari Visit, Service Execution, Invoice, dan Payment Attempt. |
| Booking | Capacity revalidation pada PR-59 dipertahankan; kebijakan pending-slot hold/expiry tetap open decision. |
| Visit | PR-66 menambahkan kebutuhan pelacakan status Visit yang independen. |
| Service Execution | PR-67 menambahkan kebutuhan pelacakan status setiap pelaksanaan layanan dalam Visit. |
| Booking provenance | PR-68 menambahkan identitas, peran, dan kanal/sumber pembuat booking dari konteks terautentikasi server. |
| UX implications | Booking, Visit, Service Execution, Invoice, dan Payment ditampilkan sebagai status berbeda; tidak mengklaim UI telah diimplementasikan. |
| Release/MVP | PR-68 diusulkan dalam Release 2; PR-66/PR-67 diusulkan dalam Release 3. Prioritas final menunggu stakeholder. |
| Traceability | Penelusuran BR-57–60, MR-21/24/25, PR-59/66–68, dan SR-F-091/101–103 diperjelas. |

## Added requirements

| Requirement | Kebutuhan | SRD Mapping |
|---|---|---|
| PR-66 | Status Visit terpisah dari status operasional dan transaksi lainnya. | SR-F-101 |
| PR-67 | Status setiap Service Execution terpisah dari Visit dan layanan lainnya. | SR-F-102 |
| PR-68 | Identitas/peran/kanal pembuat booking dapat ditelusuri. | SR-F-103 |

## Requirement dipertahankan

PR-01 sampai PR-65 tetap mengikuti PRD v1.0 dan PRD Refinement v1.1 kecuali klasifikasi lifecycle yang diperjelas oleh v1.2. Secara khusus, PR-14 dan PR-59 sudah memuat capacity validation dan tidak diduplikasi dengan ID baru.

## Open decisions yang tidak diubah menjadi fakta

- Pending slot hold, expiry, serta SLA admin.
- Approval untuk booking yang dibuat Admin.
- Booking–Visit cardinality dan status/transisi Visit serta Service Execution.
- Hak akses status, mapping invoice, dan gateway retry/refund/reconciliation.

## Preservasi versi dan status

- Dokumen PRD v1.0 (PDF) tidak berubah.
- PRD Refinement v1.1 (Markdown, PDF, dan Change Log) tidak berubah.
- PRD Refinement v1.2 diterbitkan sebagai addendum terpisah.

**Status akhir:** Proposed refinement / Draft — pending stakeholder validation.

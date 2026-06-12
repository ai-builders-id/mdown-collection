# BRD: GRC — Business Requirements

**Project:** GRC (Governance Risk Compliance)
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Perusahaan menghadapi tekanan regulasi yang semakin ketat — UU ITE, UU PDP (Perlindungan Data Pribadi), standar ISO, dan regulasi sektoral. Saat ini, manajemen risiko dan kepatuhan dikelola secara terpisah di spreadsheet, email, dan dokumen fisik. Risk & Compliance Officer kesulitan memonitor status mitigasi risiko, tingkat kepatuhan terhadap regulasi, dan tindak lanjut temuan audit. GRC hadir sebagai platform terintegrasi untuk mengelola seluruh siklus governance, risk, dan compliance dalam satu dashboard.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Sentralisasi risk register | 100% risiko perusahaan tercatat dan termonitor |
| BRD-002 | Meningkatkan kepatuhan regulasi | Tingkat kepatuhan > 95% terhadap semua regulasi relevan |
| BRD-003 | Otomatisasi tracking temuan audit | 100% temuan audit memiliki tindak lanjut dan tenggat |
| BRD-004 | Visibilitas GRC untuk manajemen | Dashboard real-time untuk direksi dan komite audit |

---

## 3. Nilai (Value Proposition)

- **Single Platform GRC:** Kelola risiko, compliance, audit, dan kebijakan dalam satu platform terintegrasi.
- **Risk-Based Decision Making:** Prioritaskan mitigasi berdasarkan skor risiko (dampak x probabilitas).
- **Audit Trail:** Semua perubahan dan keputusan tercatat, siap untuk audit internal/eksternal.
- **Regulatory Confidence:** Pastikan perusahaan selalu patuh terhadap regulasi yang berlaku dengan tracking otomatis.

---

## 4. Target Pengguna

### Primary Persona: Risk & Compliance Officer

- Demografis: 28-50 tahun, latar belakang hukum/akuntansi/manajemen risiko.
- Kebutuhan utama: Identifikasi dan assessment risiko, monitor kepatuhan, tindak lanjut audit.
- Pain points: Data tersebar di banyak spreadsheet, tidak ada early warning untuk tenggat kepatuhan.

### Secondary Persona: Internal Auditor

- Demografis: 30-55 tahun, auditor internal bersertifikasi.
- Kebutuhan: Mencatat temuan audit, track tindak lanjut, generate laporan audit.
- Pain points: Temuan audit hilang/terlupakan setelah audit selesai.

### Tertiary Persona: Manajemen/Direksi

- Kebutuhan: Melihat risk dashboard, status kepatuhan, laporan GRC ringkas.
- Pain points: Tidak punya visibilitas real-time terhadap eksposur risiko perusahaan.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Risk Register | P0 | Identifikasi, assessment, mitigasi, monitoring risiko dengan skor otomatis |
| Risk Heatmap | P0 | Matriks visual 5x5 dampak vs probabilitas |
| Risk Kanban | P0 | Pipeline status risiko dengan drag & drop |
| Compliance Tracker | P0 | Daftar kewajiban kepatuhan dengan status dan tenggat |
| Audit Temuan | P1 | Pencatatan temuan audit, severity, tindak lanjut |
| Dashboard GRC | P0 | Ringkasan: total risiko, kritis, kepatuhan, temuan terbuka |
| Kebijakan & Dokumen | P2 | Repository kebijakan, versi, status |
| Laporan GRC | P1 | Export risk register, compliance report ke PDF/Excel |

---

## 6. Model Distribusi

- Static frontend boilerplate — bisa langsung diakses via browser.
- Mock data untuk 25+ risiko, 15+ compliance items, 10+ audit temuan.
- Siap diintegrasikan dengan backend untuk data real dan workflow approval.

---

## 7. KPI & Metrik Kesuksesan

- **Coverage:** 100% unit bisnis tercakup dalam risk register.
- **Timeliness:** 95% compliance items selesai sebelum tenggat.
- **Closure Rate:** 80% temuan audit closed dalam 30 hari.
- **Adoption:** 90% Risk & Compliance Officer aktif menggunakan platform dalam 1 bulan.

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | GRC Advantage |
|------------|-----------|------------|---------------------------|
| SAP GRC | Enterprise-grade, fitur lengkap | Sangat mahal (miliaran), kompleks, perlu konsultan | Gratis, ringan, fokus esensial |
| MetricStream | Market leader GRC, AI-powered | Biaya lisensi tinggi, implementasi lama | Zero setup, open-source, customizable |
| Spreadsheet | Gratis, familiar | Mudah korup, tidak ada audit trail, tidak kolaboratif | Real-time, audit trail, kolaborasi |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Skor risiko tidak akurat | Prioritas mitigasi salah | Formula standar (dampak x probabilitas), panduan penilaian |
| Data compliance tidak update | Pelanggaran regulasi tidak terdeteksi | Reminder notifikasi, status tracking visual |
| Resistensi penggunaan | Data tidak terpusat | UI intuitif, import spreadsheet existing |

---

## 10. Rencana Implementasi

1. Fase 1 — Risk register, heatmap, kanban board dengan mock data.
2. Fase 2 — Compliance tracker, audit temuan, dashboard.
3. Fase 3 — Kebijakan repository, laporan export (PDF/Excel).
4. Fase 4 — Polish UI/UX, responsive design, testing.

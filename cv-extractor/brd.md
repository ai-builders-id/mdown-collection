# BRD: CV Extractor — Business Requirements

**Project:** CV Extractor
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Proses rekrutmen di banyak perusahaan masih melibatkan review CV secara manual satu per satu. HR Recruiter harus membaca puluhan bahkan ratusan CV untuk mencari kandidat yang sesuai, kemudian menyalin data penting ke sistem ATS (Applicant Tracking System) secara manual. Ini memakan waktu 5-10 menit per CV dan rentan terhadap human error dalam mentransfer data. CV Extractor hadir untuk mengotomatisasi ekstraksi data CV ke format terstruktur sehingga recruiter bisa fokus pada evaluasi kandidat, bukan entry data.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Otomatisasi entry data CV | Waktu ekstraksi per CV turun dari 10 menit menjadi 30 detik |
| BRD-002 | Meningkatkan akurasi data kandidat | Akurasi ekstraksi data > 90% untuk field utama |
| BRD-003 | Mempercepat pipeline rekrutmen | Waktu screening CV turun 60% |
| BRD-004 | Standardisasi data kandidat | 100% data kandidat memiliki format yang seragam |

---

## 3. Nilai (Value Proposition)

- **Otomatisasi Penuh:** Ekstraksi nama, kontak, pengalaman, pendidikan, skill secara otomatis dari CV.
- **Format Terstandarisasi:** Semua data kandidat dalam format yang konsisten dan siap diintegrasikan ke sistem lain.
- **Hemat Waktu:** Recruiter bisa memproses 10x lebih banyak CV dalam waktu yang sama.
- **Ekspor Fleksibel:** Data bisa diexport ke JSON, CSV, atau langsung di-copy ke clipboard.

---

## 4. Target Pengguna

### Primary Persona: HR Recruiter

- Demografis: 23-40 tahun, pendidikan S1, staf HR/spesialis rekrutmen.
- Kebutuhan utama: Memproses CV kandidat cepat, mendapatkan data terstruktur, mengintegrasikan dengan ATS.
- Pain points: Entry data manual membosankan, sering salah copy, format CV tidak konsisten.

### Secondary Persona: Talent Acquisition Manager

- Demografis: 30-50 tahun, manajer tim rekrutmen.
- Kebutuhan: Melihat pipeline kandidat, analisis skill gap, laporan rekrutmen.
- Pain points: Sulit mendapatkan data agregat dari CV karena format berbeda-beda.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Upload CV (drag & drop) | P0 | Upload file PDF/DOCX/TXT melalui drag & drop atau file picker |
| Mock CV Library | P0 | 5-10 CV contoh siap pakai untuk demo dan testing |
| Ekstraksi Data Pribadi | P0 | Nama, email, telepon, alamat, LinkedIn |
| Ekstraksi Pengalaman | P0 | Riwayat pekerjaan dengan tanggal, posisi, perusahaan |
| Ekstraksi Pendidikan | P0 | Riwayat pendidikan dengan gelar, institusi, IPK |
| Ekstraksi Skill & Sertifikasi | P0 | Daftar skill teknis dan non-teknis, sertifikasi |
| Preview Hasil Terstruktur | P0 | Form read-only menampilkan semua hasil parsing |
| Export JSON | P1 | Download data hasil ekstraksi dalam format JSON |

---

## 6. Model Distribusi

- Static frontend boilerplate — buka di browser, langsung bisa digunakan.
- Mock CV data siap pakai untuk demonstrasi tanpa perlu upload file real.
- Siap diintegrasikan dengan backend API untuk parsing CV real menggunakan NLP/ML.

---

## 7. KPI & Metrik Kesuksesan

- **Kecepatan:** Waktu dari upload hingga hasil tampil < 3 detik (simulasi).
- **Akurasi:** Data yang berhasil diekstrak > 90% akurat dibanding CV asli.
- **Adoption:** 80% recruiter menggunakan CV Extractor dalam proses screening.
- **Efisiensi:** Pengurangan waktu entry data CV sebesar 80%.

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | CV Extractor Advantage |
|------------|-----------|------------|---------------------------|
| Sovren | API parsing akurat (95%+) | Berbayar ($0.25-0.50 per CV), tidak transparan | Gratis, open-source, transparan |
| Rchilli | Support banyak format CV | Mahal, enterprise-only | Tidak ada biaya, siap pakai lokal |
| Affinda | UI bagus, akurat | Terbatas untuk enterprise | UI modern, mock data lengkap, open-source |
| Manual Copy-Paste | Gratis, tanpa setup | Lambat, rawan error | Cepat, akurat, terstandarisasi |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Format CV tidak standar | Ekstraksi tidak akurat | Support multiple format, fallback ke manual input |
| Data sensitif kandidat | Pelanggaran privasi | Semua data in-memory, tidak disimpan, mock data only |
| Harapan parsing real vs mock | User kecewa | Label jelas "Demo Mode — Simulasi Parsing" |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe dengan mock data & CV library (1 minggu).
2. Fase 2 — UI polish, animations, export functionality (1 minggu).
3. Fase 3 — Validasi dengan recruiter, feedback loop (1 minggu).
4. Fase 4 — Dokumentasi dan persiapan integrasi API (1 minggu).

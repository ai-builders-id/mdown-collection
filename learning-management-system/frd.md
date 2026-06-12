# FRD: Learning Management System — Functional Requirements

**Project:** Learning Management System
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk Learning Management System — aplikasi manajemen kursus online untuk Siswa/Pelatih.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Katalog Kursus

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Grid kartu kursus dengan thumbnail, judul, rating, instruktur, level | P0 |
| FRD-002 | Filter berdasarkan kategori, tingkat kesulitan, harga (gratis/berbayar) | P0 |
| FRD-003 | Pencarian berdasarkan judul kursus | P0 |
| FRD-004 | Sorting berdasarkan rating, popularitas, terbaru | P1 |
| FRD-005 | Halaman detail kursus (deskripsi, silabus modul, review) | P0 |

### 2.2 Modul Player Konten

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-006 | Sidebar daftar modul dengan status (selesai/sedang/belum) | P0 |
| FRD-007 | Player video dengan play/pause, progress, fullscreen (mock) | P0 |
| FRD-008 | Konten bacaan dengan markdown rendering | P0 |
| FRD-009 | Navigasi Previous/Next antar modul | P0 |
| FRD-010 | Tandai modul selesai (mark complete) | P0 |
| FRD-011 | Bookmark modul untuk akses cepat | P2 |

### 2.3 Modul Quiz & Evaluasi

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-012 | Tampilan quiz pilihan ganda dengan 4 opsi | P0 |
| FRD-013 | Submit jawaban dengan feedback benar/salah instan | P0 |
| FRD-014 | Penjelasan untuk setiap jawaban setelah submit | P1 |
| FRD-015 | Skor akhir setelah menyelesaikan semua soal | P1 |
| FRD-016 | Waktu pengerjaan per quiz (timer) | P2 |

### 2.4 Modul Sertifikat

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-017 | Generate sertifikat otomatis saat 100% modul selesai | P1 |
| FRD-018 | Tampilan sertifikat dengan nama siswa, judul kursus, tanggal | P1 |
| FRD-019 | Download sertifikat sebagai PDF (mock) | P1 |
| FRD-020 | Animasi confetti saat sertifikat muncul | P2 |

### 2.5 Modul Dashboard Siswa

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-021 | Kartu kursus yang sedang diambil dengan progress bar | P0 |
| FRD-022 | Statistik belajar: total jam, kursus selesai, sertifikat | P0 |
| FRD-023 | Rekomendasi kursus berdasarkan kategori yang diminati | P1 |
| FRD-024 | Aktivitas terbaru (modul terakhir dipelajari) | P1 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Mengikuti Kursus dari Awal hingga Selesai

- **Aktor:** Siswa
- **Pre-condition:** Siswa sudah terdaftar di kursus dan memiliki akses ke semua modul
- **Post-condition:** Semua modul selesai, sertifikat tersedia
- **Alur Normal:**
  1. Siswa memilih kursus dari halaman "Kursus Saya"
  2. Sistem menampilkan halaman kursus dengan sidebar modul di kiri dan konten modul pertama di kanan
  3. Siswa menonton video/membaca materi
  4. Siswa klik "Tandai Selesai" setelah selesai mempelajari modul
  5. Progress bar di bottom bar update, modul berikutnya terbuka
  6. Ulangi langkah 3-5 untuk semua modul
  7. Setelah modul terakhir selesai, sistem menampilkan modal sertifikat dengan animasi confetti
  8. Siswa dapat download sertifikat atau kembali ke dashboard
- **Alur Alternatif:**
  - A1: Modul berisi quiz — siswa harus menjawab quiz dengan minimal 60% benar untuk lanjut
  - A2: Siswa klik "Previous" untuk mereview modul sebelumnya
- **Exception:**
  - E1: Video tidak termuat — tampilkan fallback "Materi tidak tersedia" dan opsi reload

### UC-02: Mengerjakan Quiz

- **Aktor:** Siswa
- **Pre-condition:** Siswa membuka modul bertipe quiz
- **Post-condition:** Skor quiz tercatat, feedback diterima
- **Alur Normal:**
  1. Sistem menampilkan soal pertama dengan 4 opsi jawaban
  2. Siswa memilih salah satu opsi
  3. Sistem langsung menampilkan feedback: benar (hijau) atau salah (merah) + penjelasan
  4. Siswa klik "Soal Selanjutnya"
  5. Setelah semua soal terjawab, sistem menampilkan skor akhir
  6. Jika skor >= 60%, modul dianggap selesai. Jika tidak, siswa harus mengulang
- **Alur Alternatif:**
  - A1: Timer habis — quiz otomatis submit dengan jawaban yang sudah dipilih

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Prasyarat modul | Modul hanya bisa diakses jika modul sebelumnya sudah selesai |
| BR-002 | Nilai minimal quiz | Skor minimal 60% untuk menyelesaikan modul quiz |
| BR-003 | Pengulangan quiz | Siswa bisa mengulang quiz maksimal 3 kali |
| BR-004 | Sertifikat otomatis | Sertifikat terbit otomatis saat semua modul dalam kursus selesai |
| BR-005 | Progress real-time | Progress bar update setiap kali modul ditandai selesai |

---

## 5. Integrasi & Data Flow

```text
[Katalog] --> [Detail Kursus] --> [Player Konten] --> [Progress Update]
                                   |                       |
                                   v                       v
                              [Quiz System] --> [Score Tracking]
                                                       |
                                                       v
                                              [Sertifikat Engine]
                                                       |
                                                       v
                                              [Dashboard Update]
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik untuk memuat halaman katalog (20+ kursus) |
| NFR-002 | Responsive | Desktop + Tablet + Mobile |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Katalog Kursus | High | Low | P0 |
| Player Konten | High | High | P0 |
| Progress Tracking | High | Low | P0 |
| Quiz Sederhana | High | Medium | P1 |
| Sertifikat | Medium | Medium | P1 |
| Dashboard | Medium | Low | P0 |
| Forum Diskusi | Low | High | P2 |
| Bookmark | Low | Low | P2 |

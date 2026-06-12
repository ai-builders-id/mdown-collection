# BRD: Learning Management System — Business Requirements

**Project:** Learning Management System
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Perusahaan/organisasi membutuhkan platform pembelajaran terpusat untuk melakukan training dan upskilling karyawan. Saat ini, materi training tersebar dalam berbagai format (PDF, video Google Drive, link eksternal) tanpa sistem pelacakan progress yang terpusat. Manajer kesulitan memantau kompetensi tim, dan karyawan tidak memiliki jalur belajar yang terstruktur. LMS ini akan menyediakan platform kursus online dengan tracking progress, quiz, dan sertifikat otomatis.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Menyediakan platform pembelajaran terpusat | 100% materi training dalam satu platform |
| BRD-002 | Meningkatkan kompetensi karyawan | 80% karyawan menyelesaikan minimal 1 kursus per kuartal |
| BRD-003 | Otomatisasi sertifikasi | Waktu penerbitan sertifikat dari 3 hari menjadi real-time |
| BRD-004 | Tracking progress learning | 100% progress learning terpantau real-time |

---

## 3. Nilai (Value Proposition)

- **Pembelajaran Terstruktur:** Jalur belajar (learning path) yang jelas dari pemula hingga mahir.
- **Progress Tracking:** Pantau progress belajar secara real-time, per modul dan per kursus.
- **Sertifikasi Otomatis:** Sertifikat digital langsung terbit saat kursus selesai.
- **Fleksibel Akses:** Belajar kapan saja, di mana saja, dari perangkat apa saja.

---

## 4. Target Pengguna

### Primary Persona: Siswa/Peserta Kursus

- Demografis: 20-45 tahun, karyawan/profesional, ingin mengembangkan skill.
- Kebutuhan utama: Belajar dengan materi terstruktur, tracking progress, dapat sertifikat.
- Pain points: Materi berserakan, tidak ada panduan belajar, lupa progress belajar.

### Secondary Persona: Instruktur/Pelatih

- Demografis: 30-55 tahun, ahli di bidangnya, trainer internal/eksternal.
- Kebutuhan: Membuat dan mengelola konten kursus, melihat performa siswa.
- Pain points: Sulit memantau pemahaman siswa, feedback tidak terstruktur.

### Tertiary Persona: Manajer Learning & Development

- Kebutuhan: Melihat laporan training, analisis skill gap tim, ROI pelatihan.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Katalog Kursus | P0 | Daftar kursus dengan filter kategori, tingkat, rating |
| Player Konten | P0 | Video embed, materi bacaan, navigasi modul |
| Progress Tracking | P0 | Progress bar per kursus, status per modul |
| Quiz & Evaluasi | P1 | Quiz pilihan ganda dengan feedback instan dan skor |
| Sertifikat Digital | P1 | Generate sertifikat otomatis saat kursus selesai |
| Dashboard Siswa | P0 | Ringkasan kursus aktif, progress, rekomendasi |
| Forum Diskusi | P2 | Thread diskusi per modul untuk tanya jawab |

---

## 6. Model Distribusi

- Static frontend boilerplate — dapat diakses via browser.
- Mock data mencakup 8+ kursus, 50+ modul, 3 level kesulitan.
- Siap diintegrasikan dengan backend REST API untuk user management dan konten dinamis.

---

## 7. KPI & Metrik Kesuksesan

- **Completion Rate:** > 70% siswa menyelesaikan kursus yang dimulai.
- **Engagement:** Rata-rata 30 menit/sesi belajar.
- **Satisfaction:** Rating kursus rata-rata > 4.5 dari 5.
- **Certification:** 60% siswa yang lulus mendownload sertifikat.

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | LMS Advantage |
|------------|-----------|------------|---------------------------|
| Moodle | Gratis, fitur lengkap, open-source | UI kuno, kompleks, perlu hosting | UI modern, zero setup, mobile-friendly |
| Ruangguru | Konten berkualitas, gamification | Berbayar, fokus K-12 | Fokus professional/karyawan, harga gratis |
| Google Classroom | Gratis, terintegrasi GSuite | Terbatas untuk kursus formal, tidak ada sertifikasi | Sertifikat otomatis, quiz built-in, tracking detail |
| Coursera | Kualitas konten tinggi, mitra universitas | Mahal ($39+/bulan), tidak customizable | Gratis, customizable, dapat diisi konten internal |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Konten tidak engaging | Siswa dropout tinggi | Materi interaktif (video + quiz), progress tracking | 
| Siswa curang saat quiz | Sertifikat tidak valid | Soal diacak, timer per quiz (untuk implementasi real) |
| Teknologi tidak compatible | Akses terbatas | Browser modern, responsive design, PWA-ready |

---

## 10. Rencana Implementasi

1. Fase 1 — Layout katalog kursus, detail kursus, player konten dengan mock data.
2. Fase 2 — Quiz system, progress tracking, sertifikat generator.
3. Fase 3 — Dashboard siswa, forum diskusi, rekomendasi kursus.
4. Fase 4 — Polish UI/UX, responsive, testing.

# BRD: Healthcare EMR — Business Requirements

**Project:** Healthcare EMR
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Rumah sakit dan klinik masih banyak yang menggunakan rekam medis berbasis kertas. Hal ini menyebabkan masalah serius: data pasien sulit dilacak, riwayat pengobatan tidak terdokumentasi dengan baik, resep hilang, dan waktu tunggu pasien lama karena dokter harus mencari berkas fisik. Healthcare EMR (Electronic Medical Record) hadir untuk mendigitalisasi seluruh proses pencatatan medis — dari pendaftaran pasien, pencatatan diagnosis, resep obat, hingga jadwal kontrol.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Digitalisasi rekam medis | 100% data pasien dalam format digital |
| BRD-002 | Mempercepat pelayanan | Waktu pencatatan rekam medis turun 50% |
| BRD-003 | Mengurangi kesalahan medis | 0% resep hilang/tertukar karena sistem |
| BRD-004 | Riwayat pasien terlacak | 100% kunjungan pasien tercatat dan mudah diakses |

---

## 3. Nilai (Value Proposition)

- **Rekam Medis Digital:** Akses instan ke riwayat kesehatan pasien kapan saja.
- **Mengurangi Kesalahan:** Input terstruktur mengurangi risiko kesalahan diagnosis dan resep.
- **Efisiensi Waktu:** Dokter tidak perlu mencari berkas fisik, semua data tersedia dalam hitungan detik.
- **Tracking Pengobatan:** Resep dan jadwal kontrol terpantau, pasien tidak terlewat.

---

## 4. Target Pengguna

### Primary Persona: Dokter

- Demografis: 28-65 tahun, dokter umum atau spesialis di klinik/rumah sakit.
- Kebutuhan utama: Mencatat diagnosis, resep, melihat riwayat pasien, jadwal praktik.
- Pain points: Berkas rekam medis berserakan, tulisan tangan tidak terbaca, riwayat pasien tidak lengkap.

### Secondary Persona: Perawat

- Demografis: 22-45 tahun, perawat di poliklinik/IGD.
- Kebutuhan: Mencatat tanda-tanda vital, mengupdate status pasien, menyiapkan berkas.
- Pain points: Entry data ganda (tulis manual + input sistem), format tidak standar.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Data Pasien (CRUD) | P0 | Registrasi, edit, cari pasien berdasarkan nama/no RM |
| Rekam Medis Baru | P0 | Input keluhan, diagnosis (ICD-10), vital signs, catatan dokter |
| Riwayat Kunjungan | P0 | Timeline kronologis semua kunjungan pasien |
| Resep Obat | P1 | Input resep dengan dosis, frekuensi, durasi, status |
| Dashboard Dokter | P0 | Ringkasan pasien hari ini, jadwal praktik |
| Jadwal Kontrol | P1 | Kalender jadwal pasien kontrol, status kunjungan |
| Laporan Medis | P2 | Rekap diagnosis, statistik kunjungan |

---

## 6. Model Distribusi

- Static frontend boilerplate — buka via browser, siap demo.
- Mock data untuk 20+ pasien dengan 50+ kunjungan historis.
- Siap diintegrasikan dengan backend untuk integrasi SIMRS (Sistem Informasi Manajemen Rumah Sakit).

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** 90% dokter menggunakan EMR dalam 1 bulan.
- **Kecepatan:** Waktu input rekam medis per pasien < 5 menit.
- **Kelengkapan:** 100% field rekam medis terisi (diagnosis, vital signs, resep).
- **Akses:** Riwayat pasien dapat diakses dalam < 3 detik.

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | Healthcare EMR Advantage |
|------------|-----------|------------|---------------------------|
| Simrs | Fitur lengkap (IGD, rawat inap, farmasi) | UI rumit, mahal (Rp 200jt+), perlu server | UI modern, gratis, zero setup |
| SATUSEHAT | Integrasi BPJS, standar Kemenkes | Wajib integrasi, tidak untuk klinik kecil | Untuk semua skala, open-source |
| Kertas/fisik | Familiar, tidak perlu listrik | Rentan rusak/hilang, tidak bisa backup | Digital, backup, akses cepat |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data sensitif pasien bocor | Pelanggaran etika & hukum | Semua data mock, tidak ada data real |
| Resistensi dokter terhadap sistem | Adopsi rendah | UI intuitif, training singkat, shortcut keyboard |
| Integrasi dengan sistem existing | Duplikasi data | Service layer terpisah untuk integrasi mudah |

---

## 10. Rencana Implementasi

1. Fase 1 — CRUD pasien, tampilan profil, riwayat kunjungan dengan mock data.
2. Fase 2 — Form diagnosis, vital signs, resep, jadwal kontrol.
3. Fase 3 — Dashboard dokter, notifikasi, kalender.
4. Fase 4 — Laporan, statistik, polish UI/UX.

# BRD: Human Resource Management — Business Requirements

**Project:** Human Resource Management
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Perusahaan saat ini mengelola data SDM secara manual menggunakan spreadsheet dan dokumen fisik. Proses rekrutmen, pencatatan data karyawan, penggajian, serta onboarding dan offboarding tidak terpusat sehingga rawan kesalahan data, duplikasi, dan keterlambatan proses. HR Manager menghabiskan 40% waktu pada tugas administratif yang seharusnya bisa diotomatisasi. Aplikasi HRM ini dibutuhkan untuk mensentralisasi seluruh siklus hidup karyawan dalam satu platform yang terintegrasi.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Digitalisasi data SDM | 100% data karyawan terpusat dalam sistem |
| BRD-002 | Mempercepat proses rekrutmen | Waktu hire turun 30% dalam 3 bulan |
| BRD-003 | Otomatisasi payroll | Kesalahan perhitungan gaji turun ke 0% |
| BRD-004 | Efisiensi onboarding | Waktu onboarding karyawan baru turun 50% |

---

## 3. Nilai (Value Proposition)

- **Single Source of Truth:** Semua data karyawan dalam satu platform terintegrasi, menghilangkan data ganda dan inkonsistensi.
- **Otomatisasi Proses:** Pengurangan input manual hingga 60% untuk tugas administratif HR Manager.
- **Visibilitas Real-time:** Dashboard dan laporan yang memberikan insight SDM secara langsung kepada manajemen.
- **Pipeline Rekrutmen:** Visibilitas penuh terhadap status setiap lowongan dan kandidat dalam satu tampilan.

---

## 4. Target Pengguna

### Primary Persona: HR Manager

- Demografis: 30-50 tahun, pendidikan S1/S2, berpengalaman di bidang SDM 5+ tahun.
- Kebutuhan utama: Mengelola data karyawan, payroll, rekrutmen, laporan manajemen.
- Pain points: Data tersebar di banyak file Excel, proses manual memakan waktu, sulit melacak riwayat karyawan.

### Secondary Persona: Manajer Departemen

- Demografis: 28-55 tahun, kepala divisi/bagian.
- Kebutuhan: Melihat data anggota tim, mengajukan permintaan rekrutmen baru, melihat ringkasan cuti tim.
- Pain points: Tidak punya akses langsung ke data SDM, harus menghubungi HR untuk data sederhana.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Manajemen Data Karyawan | P0 | CRUD karyawan, upload foto, filter departemen, pencarian real-time |
| Dashboard SDM | P0 | Statistik: total karyawan, komposisi departemen, status kepegawaian |
| Rekrutmen Tracker | P1 | Lowongan baru, daftar pelamar, status pipeline interview (kanban) |
| Payroll Calculator | P1 | Perhitungan gaji pokok + tunjangan - potongan, histori pembayaran |
| Onboarding Checklist | P2 | Task list untuk karyawan baru, progress tracking per task |
| Offboarding Workflow | P2 | Surat resign, exit checklist, dokumen serah terima |
| Laporan SDM | P1 | Export data ke PDF/Excel, grafik komposisi departemen dan status |

---

## 6. Model Distribusi

- Static boilerplate frontend — dapat dijalankan langsung di browser tanpa server.
- Mock data untuk 50+ karyawan, 10+ lowongan aktif, 3 periode payroll historis.
- Siap diintegrasikan dengan backend REST API melalui service layer yang sudah dipisah.

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** 90% HR Manager menggunakan aplikasi dalam 2 minggu pertama setelah implementasi.
- **Engagement:** Rata-rata 5 sesi per hari per user pada jam kerja.
- **Retention:** 80% user masih aktif setelah 1 bulan penggunaan.
- **Efisiensi:** Waktu input data karyawan baru turun dari 15 menit menjadi 3 menit.
- **Akurasi:** 0% kesalahan perhitungan payroll setelah migrasi.

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | HRM Advantage |
|------------|-----------|------------|---------------------------|
| Talenta | Fitur payroll kuat, terintegrasi BPJS | Mahal (Rp 50k+/karyawan), UI kompleks | Gratis sebagai boilerplate, UI modern, open-source ready |
| KlikHR | Mobile-first, banyak fitur HR | Tidak customizable, lock-in vendor | Fully customizable, kode sumber terbuka |
| Excel/Spreadsheet | Gratis, familiar, fleksibel | Rawan error, tidak real-time, tidak ada audit trail | Otomatis, terpusat, real-time, audit trail |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data tidak akurat saat migrasi | Keputusan bisnis salah | Validasi input ketat, konfirmasi sebelum simpan, preview data |
| Resistensi user terhadap sistem baru | Adopsi rendah | UI intuitif, pelatihan singkat, onboarding flow interaktif |
| Duplikasi data karyawan | Inefisiensi dan kebingungan | Validasi NIK unik, deteksi duplikat otomatis |
| Kehilangan data | Data SDM hilang | Auto-save, konfirmasi sebelum delete, mock data ter-backup |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe statis dengan mock data (2 minggu): layout, komponen, data dummy.
2. Fase 2 — Validasi dengan HR Manager (1 minggu): demo, feedback, iterasi UI/UX.
3. Fase 3 — Integrasi backend / API (4 minggu): service layer, REST API, validasi data.
4. Fase 4 — Deployment & scaling (2 minggu): hosting, CI/CD, dokumentasi.

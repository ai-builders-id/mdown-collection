# FRD: Human Resource Management — Functional Requirements

**Project:** Human Resource Management
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk Human Resource Management — aplikasi manajemen SDM untuk HR Manager.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Dashboard

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan total karyawan, rekrutmen aktif, payroll bulan ini, dan karyawan baru | P0 |
| FRD-002 | Grafik komposisi karyawan per departemen (pie chart interaktif) | P0 |
| FRD-003 | Grafik tren rekrutmen per bulan (line chart) | P1 |
| FRD-004 | Daftar ulang tahun karyawan bulan ini | P2 |
| FRD-005 | Notifikasi aktivitas terbaru (karyawan baru, payroll selesai) | P1 |

### 2.2 Modul Karyawan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-006 | CRUD data karyawan (nama, NIK, posisi, departemen, kontak, gaji) | P0 |
| FRD-007 | Filter berdasarkan departemen, status, dan rentang tanggal masuk | P0 |
| FRD-008 | Pencarian real-time berdasarkan nama atau NIK | P0 |
| FRD-009 | Upload foto profil karyawan (mock file picker) | P1 |
| FRD-010 | Export data karyawan ke CSV/Excel | P2 |
| FRD-011 | Sorting multi-kolom pada tabel (nama, gaji, tanggal) | P0 |

### 2.3 Modul Rekrutmen

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-012 | Buat lowongan baru dengan deskripsi, persyaratan, dan jumlah | P0 |
| FRD-013 | Pipeline status visual: dibuka -> review -> interview -> offering -> ditutup | P0 |
| FRD-014 | Daftar pelamar per lowongan dengan status masing-masing | P1 |
| FRD-015 | Jadwal interview dengan calon kandidat (kalender mock) | P2 |
| FRD-016 | Drag & drop antar status pipeline | P1 |

### 2.4 Modul Payroll

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-017 | Input komponen gaji: gaji pokok, tunjangan, potongan, lembur | P0 |
| FRD-018 | Perhitungan otomatis total gaji (gaji pokok + tunjangan + lembur - potongan) | P0 |
| FRD-019 | Histori payroll per periode per karyawan | P1 |
| FRD-020 | Slip gaji view (print preview dengan layout slip gaji) | P1 |
| FRD-021 | Konfirmasi payroll sebelum status berubah menjadi dibayar | P0 |

### 2.5 Modul Onboarding & Offboarding

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-022 | Checklist onboarding otomatis untuk karyawan baru | P1 |
| FRD-023 | Progress tracking onboarding per task dengan persentase | P1 |
| FRD-024 | Checklist offboarding untuk karyawan resign | P2 |
| FRD-025 | Template surat resign dan dokumen serah terima | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Menambah Karyawan Baru

- **Aktor:** HR Manager
- **Pre-condition:** User sudah membuka halaman Karyawan dan memiliki akses tulis
- **Post-condition:** Data karyawan baru tersimpan dan muncul di tabel daftar karyawan
- **Alur Normal:**
  1. User klik tombol "Tambah Karyawan" di halaman daftar karyawan
  2. Sistem menampilkan modal form dengan field: nama, NIK, posisi, departemen, email, telepon, gaji pokok, foto
  3. User mengisi data dan klik "Simpan"
  4. Sistem validasi: semua field wajib terisi, NIK unik, email valid
  5. Data tersimpan, toast sukses "Karyawan berhasil ditambahkan", tabel refresh
- **Alur Alternatif:**
  - A1: User klik "Batal" atau klik di luar modal — modal tertutup, data tidak tersimpan
- **Exception:**
  - E1: NIK sudah terdaftar — tampilkan error "NIK {{nik}} sudah digunakan oleh karyawan lain"
  - E2: Format email tidak valid — tampilkan error "Format email tidak valid"

### UC-02: Proses Payroll Bulanan

- **Aktor:** HR Manager
- **Pre-condition:** Data karyawan dengan komponen gaji sudah terisi
- **Post-condition:** Slip gaji tersedia, status payroll terupdate, riwayat tercatat
- **Alur Normal:**
  1. User buka modul Payroll, pilih periode (bulan/tahun) dari dropdown
  2. Sistem menampilkan daftar seluruh karyawan dengan kolom: gaji pokok, tunjangan, potongan, lembur, total
  3. User dapat mengedit tunjangan/lembur per karyawan untuk periode ini
  4. User klik "Konfirmasi Payroll" untuk mengunci perhitungan
  5. Status berubah menjadi "dikonfirmasi", tombol "Cetak Slip Gaji" aktif
  6. User dapat mencetak slip gaji per karyawan
- **Alur Alternatif:**
  - A1: Ada data gaji belum lengkap (0) — sistem highlight baris merah dan tampilkan peringatan
- **Exception:**
  - E1: Payroll sudah dikonfirmasi untuk periode ini — tampilkan info "Payroll periode {{periode}} sudah diproses"

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | NIK unik | Tidak boleh ada dua karyawan dengan nomor induk yang sama |
| BR-002 | Gaji pokok minimal | Gaji pokok minimal Rp3.000.000 sesuai UMR |
| BR-003 | Status cuti otomatis | Jika durasi cuti > 30 hari berturut-turut, status otomatis menjadi "cuti" |
| BR-004 | Payroll sekali per bulan | Satu karyawan hanya bisa diproses payroll sekali dalam satu periode |
| BR-005 | Pipeline sequential | Status rekrutmen harus berurutan: tidak bisa loncat dari "review" ke "offering" |

---

## 5. Integrasi & Data Flow

```text
[Dashboard] <--> [Modul Karyawan] <--> [Modul Payroll]
                     ^                     ^
                     |                     |
              [Modul Rekrutmen]   [Onboarding/Offboarding]
                     |                     |
                     +---------------------+
                      Semua modul mengakses
                      Mock Data Layer yang sama

Data Flow:
1. Karyawan baru -> Trigger onboarding checklist
2. Karyawan resign -> Trigger offboarding workflow
3. Payroll -> Ambil data gaji dari modul Karyawan
4. Dashboard -> Aggregasi data dari semua modul
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik untuk menampilkan 100+ data karyawan |
| NFR-002 | Responsive | Desktop + Tablet + Mobile dengan breakpoint Tailwind |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA, navigasi keyboard, screen reader support |
| NFR-004 | Bahasa | Bahasa Indonesia (id) untuk seluruh UI |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| CRUD Karyawan | High | Low | P0 |
| Dashboard SDM | High | Low | P0 |
| Filter & Search | High | Low | P0 |
| Payroll Calculator | High | High | P1 |
| Rekrutmen Pipeline | High | Medium | P1 |
| Slip Gaji Print | Medium | Low | P1 |
| Onboarding Checklist | Medium | Low | P2 |
| Offboarding Workflow | Low | Medium | P2 |

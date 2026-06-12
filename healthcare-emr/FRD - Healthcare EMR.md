# FRD: Healthcare EMR — Functional Requirements

**Project:** Healthcare EMR
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk Healthcare EMR — aplikasi rekam medis elektronik untuk Dokter.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Data Pasien

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Registrasi pasien baru dengan data lengkap (nama, TTL, alamat, telepon) | P0 |
| FRD-002 | Edit data pasien yang sudah terdaftar | P0 |
| FRD-003 | Pencarian pasien berdasarkan nama atau nomor RM | P0 |
| FRD-004 | Menampilkan profil pasien (data pribadi, gol. darah, alergi, penyakit bawaan) | P0 |
| FRD-005 | Menampilkan daftar alergi pasien dengan badge merah | P0 |
| FRD-006 | Filter pasien berdasarkan status (aktif/tidak aktif) | P1 |

### 2.2 Modul Rekam Medis / Kunjungan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-007 | Mulai kunjungan baru untuk pasien yang sudah dipilih | P0 |
| FRD-008 | Input keluhan pasien (text area) | P0 |
| FRD-009 | Input diagnosis dengan ICD-10 code (mock lookup) | P0 |
| FRD-010 | Input tanda-tanda vital: TD, suhu, nadi, BB, TB | P0 |
| FRD-011 | Input catatan dokter | P0 |
| FRD-012 | Timeline kronologis semua kunjungan pasien | P0 |
| FRD-013 | Edit kunjungan yang sudah tercatat | P1 |
| FRD-014 | Status kunjungan (menunggu, dalam perawatan, selesai) | P1 |

### 2.3 Modul Resep Obat

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-015 | Tambah resep obat dengan nama obat, dosis, frekuensi, durasi, jumlah | P0 |
| FRD-016 | Daftar resep aktif per pasien | P1 |
| FRD-017 | Riwayat resep yang sudah selesai/dihentikan | P1 |
| FRD-018 | Status resep (aktif/selesai/dihentikan) | P1 |

### 2.4 Modul Jadwal

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-019 | Kalender menampilkan jadwal praktik dokter | P0 |
| FRD-020 | Tambah jadwal pasien (konsultasi/kontrol/tindakan) | P1 |
| FRD-021 | Ubah status jadwal (dijadwalkan/selesai/batal) | P1 |
| FRD-022 | Notifikasi jadwal hari ini di dashboard | P1 |

### 2.5 Modul Dashboard

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-023 | Statistik: total pasien, kunjungan hari ini, jadwal hari ini | P0 |
| FRD-024 | Daftar pasien yang dijadwalkan hari ini | P0 |
| FRD-025 | Grafik tren kunjungan per minggu/bulan | P2 |
| FRD-026 | Daftar diagnosis terbanyak (top 5) | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Mencatat Kunjungan Pasien Baru

- **Aktor:** Dokter
- **Pre-condition:** Pasien sudah terdaftar di sistem
- **Post-condition:** Kunjungan baru tercatat, resep (jika ada) tersimpan
- **Alur Normal:**
  1. Dokter cari pasien via search bar (nama/no RM)
  2. Klik pasien → profil dan riwayat kunjungan muncul
  3. Klik "Kunjungan Baru" → form kunjungan terbuka
  4. Dokter isi keluhan, diagnosis, pilih ICD-10 code, isi vital signs
  5. Jika perlu resep: klik "Tambah Resep" → isi nama obat, dosis, frekuensi, durasi
  6. Klik "Simpan" → kunjungan tercatat, muncul di timeline
- **Alur Alternatif:**
  - A1: Pasien belum terdaftar — klik "Pasien Baru" dari form kunjungan
- **Exception:**
  - E1: Field wajib belum diisi — validasi error, field yang kosong di-highlight
  - E2: Dosis obat tidak valid — peringatan "Mohon periksa dosis obat"

### UC-02: Melihat Riwayat Pasien

- **Aktor:** Dokter
- **Pre-condition:** Pasien terpilih
- **Post-condition:** Dokter melihat timeline kunjungan dan resep
- **Alur Normal:**
  1. Dokter pilih pasien dari daftar
  2. Panel tengah menampilkan data pasien, panel kanan detail
  3. Scroll timeline kunjungan — setiap entri menampilkan tanggal, diagnosis, dokter
  4. Klik entri kunjungan → detail lengkap (vital signs, catatan, resep)
- **Alur Alternatif:**
  - A1: Tidak ada riwayat — tampilkan "Belum ada kunjungan untuk pasien ini"

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | ICD-10 wajib | Setiap diagnosis harus disertai kode ICD-10 |
| BR-002 | Alergi alert | Jika resep mengandung obat yang tercatat sebagai alergi pasien, tampilkan peringatan |
| BR-003 | Satu kunjungan aktif | Dalam satu waktu, hanya boleh ada satu kunjungan aktif per pasien |
| BR-004 | Resep tidak bisa dihapus | Resep hanya bisa diubah statusnya menjadi "dihentikan", tidak bisa dihapus |
| BR-005 | Vital signs wajib | Setiap kunjungan baru minimal harus memiliki data tekanan darah dan suhu tubuh |

---

## 5. Integrasi & Data Flow

```text
[Pasien Module] <--> [Kunjungan Module] <--> [Resep Module]
       ^                     ^                      ^
       |                     |                      |
       +---------------------+----------------------+
                             |
                      [Dashboard Module]
                             |
                      [Jadwal Module]

Aliran data:
1. Pasien dipilih -> Load riwayat kunjungan + resep aktif
2. Kunjungan baru -> Update status pasien + tambah timeline
3. Resep baru -> Taut ke kunjungan + update resep aktif pasien
4. Jadwal -> Muncul di dashboard sebagai notifikasi
```

---

## 6. Non-Functional Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik untuk timeline pasien dengan 20+ kunjungan |
| NFR-002 | Responsive | Desktop + Tablet (utama) + Mobile (minimal) |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA, kontras tinggi untuk keterbacaan |
| NFR-004 | Bahasa | Bahasa Indonesia (id) untuk seluruh UI |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| CRUD Pasien | High | Low | P0 |
| Kunjungan Baru | High | Medium | P0 |
| Timeline Riwayat | High | Medium | P0 |
| Dashboard Dokter | High | Low | P0 |
| Resep Obat | High | Medium | P1 |
| Jadwal Kontrol | Medium | Medium | P1 |
| Laporan & Statistik | Low | High | P2 |
| ICD-10 Lookup Mock | Medium | Low | P1 |

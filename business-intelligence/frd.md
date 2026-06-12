# FRD: Business Intelligence — Functional Requirements

**Project:** Business Intelligence
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk Business Intelligence — aplikasi Business Intelligence (Dashboard, Visualisasi, Laporan Interaktif) untuk Manajemen dan Analis Bisnis.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Dashboard & Widget

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan grid dashboard dengan widget dalam layout yang bisa diatur | P0 |
| FRD-002 | Menampilkan widget Metric Card (label, nilai, tren, sparkline) | P0 |
| FRD-003 | Menampilkan widget Bar Chart dengan data sumbu X dan Y | P0 |
| FRD-004 | Menampilkan widget Line Chart dengan data time-series | P0 |
| FRD-005 | Menampilkan widget Pie Chart dengan data kategorikal | P0 |
| FRD-006 | Widget menampilkan tooltip data saat di-hover | P0 |
| FRD-007 | Menambahkan widget baru ke dashboard dari daftar available widgets | P1 |
| FRD-008 | Drag widget untuk mengubah posisi dalam grid | P2 |
| FRD-009 | Resize widget dalam grid (drag handle) | P2 |
| FRD-010 | Menghapus widget dari dashboard | P1 |

### 2.2 Modul Filter & Date Range

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-011 | Date range picker dengan preset (Today, Last 7 Days, Last 30 Days, This Month, Custom) | P0 |
| FRD-012 | Filter date range memengaruhi semua widget di dashboard | P0 |
| FRD-013 | Menampilkan periode yang sedang aktif di header dashboard | P1 |
| FRD-014 | Filter tambahan per widget (misal: filter kategori pada chart tertentu) | P2 |

### 2.3 Modul Laporan (Reports)

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-015 | Menampilkan daftar laporan yang sudah dibuat | P1 |
| FRD-016 | Membuat laporan baru dengan memilih widget yang akan ditampilkan | P1 |
| FRD-017 | Mengedit laporan yang sudah ada | P1 |
| FRD-018 | Menghapus laporan | P1 |
| FRD-019 | Menampilkan laporan dalam tampilan full-page tanpa sidebar | P1 |

### 2.4 Modul Data Table

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-020 | Menampilkan data dalam tabel interaktif | P1 |
| FRD-021 | Sorting data berdasarkan kolom (asc/desc) | P1 |
| FRD-022 | Pagination dengan jumlah data per halaman yang bisa diatur | P1 |
| FRD-023 | Export data ke CSV | P1 |
| FRD-024 | Pencarian data dalam tabel | P1 |

### 2.5 Modul Tema & Pengaturan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-025 | Toggle dark/light mode | P1 |
| FRD-026 | Menyimpan preferensi layout widget (posisi, ukuran) di localStorage | P2 |
| FRD-027 | Reset dashboard ke default layout | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Memonitor KPI Bisnis di Dashboard

- **Aktor:** Manajer
- **Pre-condition:** Dashboard sudah memiliki widget default dengan data
- **Post-condition:** Manajer mendapatkan gambaran performa bisnis
- **Alur Normal:**
  1. Manajer membuka halaman Dashboard
  2. Dashboard menampilkan grid widget dengan data 30 hari terakhir
  3. Manajer melihat kartu KPI (Total Revenue, Active Users, Conversion Rate)
  4. Manajer mengubah date range ke "7 Hari Terakhir"
  5. Semua widget refresh — menampilkan data 7 hari
  6. Manajer hover di chart line — tooltip menampilkan nilai detail
- **Alur Alternatif:**
  - A1: Manajer klik widget chart → diarahkan ke halaman analitik lebih detail
- **Exception:**
  - E1: Data untuk periode yang dipilih kosong — widget menampilkan empty state

### UC-02: Membuat Laporan Kustom

- **Aktor:** Analis Bisnis
- **Pre-condition:** Widget dan data sumber sudah tersedia
- **Post-condition:** Laporan baru tersimpan dan bisa diakses
- **Alur Normal:**
  1. Analis membuka halaman Laporan
  2. Analis mengklik "Buat Laporan Baru"
  3. Analis memilih widget dari daftar available (metric card, chart, data table)
  4. Analis mengatur layout dan posisi widget
  5. Analis memberi judul dan deskripsi laporan
  6. Analis menyimpan laporan — muncul di daftar laporan
- **Alur Alternatif:**
  - A1: Analis mengedit laporan yang sudah ada (tambah/hapus widget)
- **Exception:**
  - E1: Laporan tanpa widget tidak bisa disimpan

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Refresh data | Date range change memicu refresh semua widget secara simultan |
| BR-002 | Layout persist | Posisi widget disimpan di localStorage dan dipulihkan saat reload |
| BR-003 | Maksimum widget | Satu dashboard dapat memiliki maksimal 12 widget |
| BR-004 | Data sinkronisasi | Semua widget dalam dashboard merujuk pada date range yang sama |

---

## 5. Integrasi & Data Flow

```text
[Dashboard Grid] <--> [Widget Components] <--> [Mock Data]
       |                       |
       v                       v
[Date Range Filter]    [Chart Library (Recharts)]
       |
       v
[All Widgets Refresh with New Data]
```

---

## 6. Non-Functional Requirements (dari FRD)

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001 | Performa loading | < 2 detik |
| NFR-002 | Responsive | Desktop + Tablet + Mobile |
| NFR-003 | Aksesibilitas | WCAG 2.1 AA |
| NFR-004 | Bahasa | Bahasa Indonesia (id) |

---

## 7. Prioritization Matrix

| Fitur | Value | Effort | Priority |
|-------|-------|--------|----------|
| Widget Metrics & Charts | High | Medium | P0 |
| Date Range Filter | High | Low | P0 |
| Dashboard Grid Layout | High | Medium | P0 |
| Data Table Interaktif | Medium | Medium | P1 |
| Manajemen Laporan | High | High | P1 |
| Dark/Light Mode | Medium | Low | P1 |
| Drag & Resize Widget | Low | High | P2 |
| Export CSV | Medium | Low | P1 |

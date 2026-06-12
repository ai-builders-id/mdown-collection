# FRD: CRM — Functional Requirements

**Project:** CRM
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk CRM — aplikasi CRM (Kontak, Deals, Pipeline Penjualan, Aktivitas) untuk Tim Sales.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Kontak

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan daftar kontak dalam tabel (nama, email, telepon, perusahaan, status) | P0 |
| FRD-002 | Membuat kontak baru melalui form modal (nama, email, telepon, perusahaan, jabatan, tags, sumber) | P0 |
| FRD-003 | Melihat detail kontak di panel kanan dengan tabs (Info, Deals, Aktivitas) | P0 |
| FRD-004 | Mengedit data kontak | P0 |
| FRD-005 | Menghapus kontak dengan konfirmasi | P1 |
| FRD-006 | Filter kontak berdasarkan status (lead, qualified, customer, churned) | P0 |
| FRD-007 | Pencarian kontak berdasarkan nama, email, atau telepon | P0 |
| FRD-008 | Filter kontak berdasarkan tags | P1 |

### 2.2 Modul Perusahaan

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-009 | Menampilkan daftar perusahaan dalam tabel | P0 |
| FRD-010 | Membuat perusahaan baru (nama, industri, ukuran, website, alamat) | P0 |
| FRD-011 | Melihat detail perusahaan dengan daftar kontak dan deal terkait | P0 |
| FRD-012 | Mengedit data perusahaan | P0 |
| FRD-013 | Menghapus perusahaan | P1 |

### 2.3 Modul Deal & Pipeline

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-014 | Menampilkan pipeline kanban dengan stage: Lead In, Qualified, Proposal, Negotiation, Closed Won, Closed Lost | P0 |
| FRD-015 | Membuat deal baru (judul, nilai, stage, kontak, perusahaan, expected close date) | P0 |
| FRD-016 | Drag-and-drop deal antar stage | P0 |
| FRD-017 | Mengedit deal (nilai, stage, probabilitas, expected close date) | P0 |
| FRD-018 | Menghapus deal | P1 |
| FRD-019 | Menampilkan total nilai pipeline per stage | P0 |
| FRD-020 | Menampilkan daftar deal dalam tampilan tabel | P1 |

### 2.4 Modul Aktivitas

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-021 | Menampilkan timeline aktivitas per kontak/deal | P0 |
| FRD-022 | Mencatat aktivitas baru (tipe: call, email, meeting, demo, task, note) | P0 |
| FRD-023 | Mengedit aktivitas | P1 |
| FRD-024 | Menandai aktivitas sebagai selesai | P1 |
| FRD-025 | Filter aktivitas berdasarkan tipe dan tanggal | P1 |

### 2.5 Modul Dashboard

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-026 | Menampilkan ringkasan pipeline (total deals, total value, win rate) | P1 |
| FRD-027 | Menampilkan grafik pipeline value per stage | P1 |
| FRD-028 | Menampilkan aktivitas terbaru (feed) | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Mengelola Pipeline Penjualan

- **Aktor:** Sales Representative
- **Pre-condition:** Deal sudah dibuat di pipeline
- **Post-condition:** Deal berpindah stage sesuai progres
- **Alur Normal:**
  1. Sales membuka halaman Pipeline
  2. Sales melihat kanban board dengan 6 stage penjualan
  3. Sales melihat deal "Enterprise Package - PT Maju Jaya" di stage "Qualified"
  4. Sales melakukan demo dengan prospek
  5. Sales drag deal dari "Qualified" ke "Proposal"
  6. Sistem menampilkan notifikasi sukses dan total nilai pipeline di stage Proposal bertambah
- **Alur Alternatif:**
  - A1: Sales mengklik deal untuk melihat detail dan aktivitas terkait
  - A2: Sales mengedit nilai deal atau expected close date di modal detail
- **Exception:**
  - E1: Stage "Closed Lost" membutuhkan alasan kegagalan — form muncul sebelum drop

### UC-02: Mencatat Aktivitas Follow-up

- **Aktor:** Sales Representative
- **Pre-condition:** Kontak sudah ada di sistem
- **Post-condition:** Aktivitas tercatat di timeline kontak
- **Alur Normal:**
  1. Sales membuka detail kontak "Budi Santoso"
  2. Sales pindah ke tab "Aktivitas"
  3. Sales mengklik "Catat Aktivitas"
  4. Sales memilih tipe "Call", mengisi deskripsi "Follow-up proposal", set status "Selesai"
  5. Sales submit — aktivitas muncul di timeline
  6. Field "Last Activity" di kontak terupdate
- **Alur Alternatif:**
  - A1: Sales membuat aktivitas dengan status "Planned" sebagai reminder
  - A2: Sales menghubungkan aktivitas ke deal tertentu
- **Exception:**
  - E1: Aktivitas tanpa deskripsi tidak bisa disimpan

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | Stage progression | Deal hanya bisa maju ke stage berikutnya (tidak bisa mundur kecuali dari Negotiation ke Proposal) |
| BR-002 | Probabilitas default | Setiap stage memiliki probabilitas default: Lead In (10%), Qualified (30%), Proposal (50%), Negotiation (70%), Closed Won (100%), Closed Lost (0%) |
| BR-003 | Wajib alasan | Deal yang dipindah ke "Closed Lost" wajib mengisi alasan kegagalan |
| BR-004 | Unique email | Satu email kontak tidak boleh duplikat dalam sistem |

---

## 5. Integrasi & Data Flow

```text
[Sidebar Navigation] --> [Contact/Company/Deal List] --> [Detail Panel]
                              |                                |
                              v                                v
                        [Filter/Search]                [Activity Timeline]
                                                              |
                              [Pipeline Kanban]               |
                              [Drag & Drop]                  v
                              [Stage Update]           [Activity Form]
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
| CRUD Kontak | High | Medium | P0 |
| Pipeline Kanban (Drag & Drop) | High | High | P0 |
| Manajemen Deal | High | Medium | P0 |
| Catatan Aktivitas | High | Medium | P0 |
| Detail Panel dengan Tabs | High | Medium | P0 |
| Filter & Search | High | Low | P0 |
| Dashboard Sales | Medium | Medium | P1 |
| Laporan Pipeline | Low | Medium | P2 |

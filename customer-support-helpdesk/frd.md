# FRD: Customer Support & Helpdesk — Functional Requirements

**Project:** Customer Support & Helpdesk
**Version:** 1.0

---

## 1. Scope

Dokumen ini mendefinisikan functional requirements untuk Customer Support & Helpdesk — aplikasi Customer Support & Helpdesk (Tiket, Prioritas, Workflow) untuk Tim Support Pelanggan.

Semua requirement ditulis dalam format terstruktur dengan ID untuk backlog tracking.

---

## 2. Modul & Fitur

### 2.1 Modul Manajemen Tiket

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-001 | Menampilkan daftar semua tiket dalam tabel dengan sorting | P0 |
| FRD-002 | Membuat tiket baru melalui form modal dengan field wajib (judul, deskripsi, kategori, prioritas) | P0 |
| FRD-003 | Melihat detail tiket di panel terpisah (kanan) | P0 |
| FRD-004 | Mengedit tiket (judul, deskripsi, kategori, prioritas, tags) | P0 |
| FRD-005 | Mengubah status tiket (Open, In Progress, Waiting Customer, Resolved, Closed) | P0 |
| FRD-006 | Menghapus tiket dengan konfirmasi | P1 |
| FRD-007 | Menampilkan riwayat perubahan status tiket | P1 |

### 2.2 Modul Filter & Pencarian

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-008 | Filter tiket berdasarkan status (multi-select) | P0 |
| FRD-009 | Filter tiket berdasarkan prioritas | P0 |
| FRD-010 | Filter tiket berdasarkan kategori | P0 |
| FRD-011 | Pencarian tiket berdasarkan nomor tiket, judul, atau nama pelanggan | P0 |
| FRD-012 | Filter tiket berdasarkan assignee | P1 |
| FRD-013 | Filter tiket berdasarkan rentang tanggal | P1 |

### 2.3 Modul Message Thread

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-014 | Menampilkan thread pesan kronologis dalam panel detail tiket | P0 |
| FRD-015 | Menambahkan balasan pesan ke tiket | P0 |
| FRD-016 | Menambahkan internal note (hanya terlihat oleh agent) | P0 |
| FRD-017 | Menampilkan identitas pengirim pesan (customer/agent/system) | P1 |
| FRD-018 | Melampirkan file pada pesan (simulasi) | P1 |

### 2.4 Modul Dashboard

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-019 | Menampilkan kartu metrik: Total Tiket, Open, In Progress, Resolved | P1 |
| FRD-020 | Menampilkan grafik tren tiket per hari (bar chart) | P1 |
| FRD-021 | Menampilkan daftar tiket urgent yang belum di-assign | P1 |
| FRD-022 | Menampilkan statistik performa agent (resolved today, avg response) | P2 |

### 2.5 Modul Knowledge Base Internal

| ID | Requirement | Prioritas |
|----|-------------|-----------|
| FRD-023 | Menampilkan daftar artikel knowledge base | P1 |
| FRD-024 | Membuat artikel baru (judul, konten, kategori, tags) | P1 |
| FRD-025 | Mencari artikel berdasarkan judul atau tags | P1 |
| FRD-026 | Menampilkan rating artikel (helpful/not helpful) | P2 |

---

## 3. Use Case / Alur Pengguna

### UC-01: Menangani Tiket Masuk

- **Aktor:** Agent Support
- **Pre-condition:** Ada tiket baru yang masuk ke sistem
- **Post-condition:** Tiket direspons dan diproses
- **Alur Normal:**
  1. Agent melihat tiket baru di dashboard atau filter "Open"
  2. Agent klik tiket — panel detail terbuka di kanan
  3. Agent membaca deskripsi dan thread pesan
  4. Agent mengubah status menjadi "In Progress"
  5. Agent mengetik balasan dan mengirim
  6. Agent mengubah status menjadi "Resolved"
- **Alur Alternatif:**
  - A1: Agent membutuhkan info tambahan → ubah status ke "Waiting Customer"
  - A2: Agent menambahkan internal note untuk kolaborasi dengan tim
- **Exception:**
  - E1: Pelanggan membalas setelah tiket resolved → status kembali ke "Open"

### UC-02: Membuat Tiket Baru

- **Aktor:** Agent Support (atas nama pelanggan)
- **Pre-condition:** Agent menerima laporan dari pelanggan via telepon/email
- **Post-condition:** Tiket baru tercatat dalam sistem
- **Alur Normal:**
  1. Agent mengklik tombol "+ New Ticket"
  2. Modal form muncul dengan field: pelanggan, judul, deskripsi, kategori, prioritas
  3. Agent mengisi form
  4. Agent submit — tiket baru muncul di daftar tiket
  5. Agent bisa langsung merespon atau assign ke agent lain
- **Alur Alternatif:**
  - A1: Agent mencari pelanggan existing berdasarkan email, tidak ditemukan → buat kontak baru
- **Exception:**
  - E1: Field wajib (judul, deskripsi) kosong — form tidak bisa disubmit

---

## 4. Aturan Bisnis (Business Rules)

| ID | Rule | Deskripsi |
|----|------|-----------|
| BR-001 | SLA Prioritas | Urgent: response < 15 menit; High: < 1 jam; Medium: < 4 jam; Low: < 24 jam |
| BR-002 | Auto-escalation | Tiket Urgent yang tidak direspons dalam 30 menit otomatis naik ke manager |
| BR-003 | Status flow | Tiket tidak bisa langsung ke Closed dari Open — harus melalui In Progress |
| BR-004 | Duplikasi | Sistem mendeteksi judul/email pelanggan yang sama dalam 24 jam sebagai potensi duplikasi |

---

## 5. Integrasi & Data Flow

```text
[Ticket List] <--> [Ticket Detail] <--> [Message Thread]
      |                                      |
      v                                      v
[Filter/Search]                        [Knowledge Base]
      |
      v
[Dashboard Stats] <--> [Chart Data (Mock)]
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
| CRUD Tiket + Status Workflow | High | Medium | P0 |
| Filter & Pencarian Tiket | High | Medium | P0 |
| Message Thread & Balasan | High | Medium | P0 |
| Dashboard Statistik | High | Medium | P1 |
| Knowledge Base Internal | Medium | Medium | P1 |
| Laporan & Ekspor CSV | Medium | High | P2 |
| Assignment Tiket | High | Low | P1 |
